<script setup lang="ts">
import { ref, onMounted, watch, defineProps } from 'vue'
import { useFileDialog } from '@vueuse/core'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

const props = defineProps(['user'])

const newProject = ref({
  name: '',
  description: '',
  priority: 'medium',
  location: null as { lat: number; lng: number } | null,
  images: [] as string[],
  manualLat: '',
  manualLng: ''
})

const projects = ref([
  {
    id: 1,
    name: 'إعادة تأهيل جسر الفرات',
    description: 'إصلاح الأضرار في الجسر الرئيسي وتقوية الدعائم',
    priority: 'high',
    status: 'pending',
    location: { lat: 35.8375, lng: 38.9967 },
    images: [],
    createdBy: 'وزارة النقل'
  }
])

const priorities = [
  { title: 'عالية', value: 'high', color: 'error' },
  { title: 'متوسطة', value: 'medium', color: 'warning' },
  { title: 'منخفضة', value: 'low', color: 'success' }
]

const mapCenter = ref({ lat: 35.8375, lng: 38.9967 })
let map: L.Map
let marker: L.Marker

const { open, onChange } = useFileDialog({
  accept: 'image/*',
  multiple: true
})

onChange((files) => {
  if (!files) return
  
  Array.from(files).forEach(file => {
    const reader = new FileReader()
    reader.onload = (e) => {
      if (e.target?.result) {
        newProject.value.images.push(e.target.result as string)
      }
    }
    reader.readAsDataURL(file)
  })
})

// Watch for manual coordinate changes
watch([() => newProject.value.manualLat, () => newProject.value.manualLng], ([newLat, newLng]) => {
  if (newLat && newLng) {
    const lat = parseFloat(newLat)
    const lng = parseFloat(newLng)
    
    if (!isNaN(lat) && !isNaN(lng) && lat >= -90 && lat <= 90 && lng >= -180 && lng <= 180) {
      newProject.value.location = { lat, lng }
      
      if (marker) {
        marker.setLatLng([lat, lng])
      } else {
        marker = L.marker([lat, lng]).addTo(map)
      }
      
      map.setView([lat, lng], map.getZoom())
    }
  }
})

onMounted(() => {
  // Initialize map
  map = L.map('map').setView([mapCenter.value.lat, mapCenter.value.lng], 7)
  
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
  }).addTo(map)

  // Add click handler to map
  map.on('click', (e) => {
    const { lat, lng } = e.latlng
    newProject.value.location = { lat, lng }
    newProject.value.manualLat = lat.toFixed(6)
    newProject.value.manualLng = lng.toFixed(6)
    
    if (marker) {
      marker.setLatLng([lat, lng])
    } else {
      marker = L.marker([lat, lng]).addTo(map)
    }
  })
})

const handleAddProject = () => {
  if (!newProject.value.location) {
    alert('الرجاء تحديد موقع المشروع على الخريطة أو إدخال الإحداثيات')
    return
  }

  projects.value.push({
    id: projects.value.length + 1,
    ...newProject.value,
    status: 'pending',
    createdBy: props.user?.name || 'مستخدم غير معروف'
  })

  // Reset form
  newProject.value = {
    name: '',
    description: '',
    priority: 'medium',
    location: null,
    images: [],
    manualLat: '',
    manualLng: ''
  }

  if (marker) {
    marker.remove()
    marker = undefined as any
  }
}

const removeImage = (index: number) => {
  newProject.value.images.splice(index, 1)
}

const getPriorityColor = (priority: string) => {
  return priorities.find(p => p.value === priority)?.color || 'grey'
}

// Check if user can create projects
const canCreateProjects = () => {
  return props.user && [2, 3, 4, 5].includes(props.user.role) // Government, Organization, Contractor, Evaluation
}

// Check if user can manage projects
const canManageProjects = () => {
  return props.user && [2, 5].includes(props.user.role) // Government, Evaluation
}
</script>

<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <div class="d-flex align-center mb-4">
          <h1 class="text-h4">إدارة المشاريع</h1>
          <v-spacer></v-spacer>
          <v-chip
            v-if="user"
            :color="user.role === 2 ? 'success' : 'primary'"
            variant="outlined"
          >
            {{ user.type }}
          </v-chip>
        </div>
      </v-col>
    </v-row>

    <!-- Add New Project Form - Only for authorized users -->
    <v-row v-if="canCreateProjects()">
      <v-col cols="12">
        <v-card class="mb-6">
          <v-card-title>
            <v-icon icon="mdi-plus-circle" class="ml-2"></v-icon>
            مشروع جديد
          </v-card-title>
          <v-card-text>
            <v-form @submit.prevent="handleAddProject">
              <v-row>
                <v-col cols="12" md="6">
                  <v-text-field
                    v-model="newProject.name"
                    label="اسم المشروع"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12" md="6">
                  <v-select
                    v-model="newProject.priority"
                    :items="priorities"
                    item-title="title"
                    item-value="value"
                    label="الأولوية"
                    required
                  ></v-select>
                </v-col>
                <v-col cols="12">
                  <v-textarea
                    v-model="newProject.description"
                    label="وصف المشروع"
                    required
                  ></v-textarea>
                </v-col>
              </v-row>

              <!-- Manual Coordinates Input -->
              <v-row class="mb-4">
                <v-col cols="12" md="6">
                  <v-text-field
                    v-model="newProject.manualLat"
                    label="خط العرض"
                    type="number"
                    step="0.000001"
                    min="-90"
                    max="90"
                    hint="مثال: 35.8375"
                    persistent-hint
                  ></v-text-field>
                </v-col>
                <v-col cols="12" md="6">
                  <v-text-field
                    v-model="newProject.manualLng"
                    label="خط الطول"
                    type="number"
                    step="0.000001"
                    min="-180"
                    max="180"
                    hint="مثال: 38.9967"
                    persistent-hint
                  ></v-text-field>
                </v-col>
              </v-row>

              <!-- Map -->
              <div class="mb-4">
                <div id="map" style="height: 400px; width: 100%; border-radius: 4px;"></div>
                <div class="text-caption mt-1">
                  انقر على الخريطة لتحديد موقع المشروع أو أدخل الإحداثيات يدوياً
                  <template v-if="newProject.location">
                    ({{ newProject.location.lat.toFixed(6) }}, {{ newProject.location.lng.toFixed(6) }})
                  </template>
                </div>
              </div>

              <!-- Image Upload -->
              <div class="mb-4">
                <v-btn
                  color="primary"
                  variant="outlined"
                  prepend-icon="mdi-camera"
                  @click="open()"
                >
                  إضافة صور للأضرار
                </v-btn>

                <v-row class="mt-2">
                  <v-col
                    v-for="(image, index) in newProject.images"
                    :key="index"
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-card>
                      <v-img
                        :src="image"
                        height="200"
                        cover
                      ></v-img>
                      <v-card-actions>
                        <v-btn
                          color="error"
                          variant="text"
                          icon
                          @click="removeImage(index)"
                        >
                          <v-icon>mdi-delete</v-icon>
                        </v-btn>
                      </v-card-actions>
                    </v-card>
                  </v-col>
                </v-row>
              </div>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="primary"
                  type="submit"
                >
                  إضافة المشروع
                </v-btn>
              </v-card-actions>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Access Denied Message for unauthorized users -->
    <v-row v-else>
      <v-col cols="12">
        <v-alert
          type="info"
          class="mb-6"
        >
          <v-alert-title>صلاحيات محدودة</v-alert-title>
          نوع حسابك ({{ user?.type }}) لا يسمح بإنشاء مشاريع جديدة. يمكنك فقط عرض المشاريع الموجودة.
        </v-alert>
      </v-col>
    </v-row>

    <!-- Projects List -->
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>
            <v-icon icon="mdi-format-list-bulleted" class="ml-2"></v-icon>
            قائمة المشاريع
          </v-card-title>
          <v-card-text>
            <v-table>
              <thead>
                <tr>
                  <th>اسم المشروع</th>
                  <th>الوصف</th>
                  <th>الأولوية</th>
                  <th>الموقع</th>
                  <th>الحالة</th>
                  <th>أنشئ بواسطة</th>
                  <th v-if="canManageProjects()">الإجراءات</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="project in projects" :key="project.id">
                  <td>{{ project.name }}</td>
                  <td>{{ project.description }}</td>
                  <td>
                    <v-chip
                      :color="getPriorityColor(project.priority)"
                      size="small"
                    >
                      {{ priorities.find(p => p.value === project.priority)?.title }}
                    </v-chip>
                  </td>
                  <td>
                    <span class="text-caption">
                      {{ project.location.lat.toFixed(6) }}, {{ project.location.lng.toFixed(6) }}
                    </span>
                  </td>
                  <td>
                    <v-chip
                      color="grey"
                      size="small"
                    >
                      قيد المراجعة
                    </v-chip>
                  </td>
                  <td>{{ project.createdBy }}</td>
                  <td v-if="canManageProjects()">
                    <v-btn
                      icon
                      variant="text"
                      color="primary"
                      size="small"
                    >
                      <v-icon>mdi-pencil</v-icon>
                    </v-btn>
                    <v-btn
                      icon
                      variant="text"
                      color="error"
                      size="small"
                    >
                      <v-icon>mdi-delete</v-icon>
                    </v-btn>
                  </td>
                </tr>
              </tbody>
            </v-table>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<style>
@import 'leaflet/dist/leaflet.css';
</style>