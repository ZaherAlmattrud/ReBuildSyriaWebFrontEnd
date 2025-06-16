<script setup lang="ts">
import { ref, computed, defineProps } from 'vue'

const props = defineProps(['user'])

// Mock data for citizen dashboard
const proposedProjects = ref([
  {
    id: 1,
    name: 'إعادة تأهيل جسر الفرات',
    description: 'إصلاح الأضرار في الجسر الرئيسي وتقوية الدعائم',
    location: 'حلب',
    votes: 245,
    hasVoted: false,
    priority: 'high'
  },
  {
    id: 2,
    name: 'ترميم مدرسة الشهداء',
    description: 'إعادة بناء الفصول الدراسية وتجهيز المختبرات',
    location: 'دمشق',
    votes: 189,
    hasVoted: true,
    priority: 'medium'
  },
  {
    id: 3,
    name: 'إصلاح شبكة المياه',
    description: 'إعادة تأهيل شبكة توزيع المياه في الحي',
    location: 'حمص',
    votes: 156,
    hasVoted: false,
    priority: 'high'
  }
])

const activeProjects = ref([
  {
    id: 4,
    name: 'بناء مركز صحي',
    description: 'إنشاء مركز صحي جديد لخدمة المنطقة',
    location: 'اللاذقية',
    progress: 75,
    contractor: 'شركة البناء المتقدم',
    startDate: '2024-01-15',
    expectedEnd: '2024-06-30'
  },
  {
    id: 5,
    name: 'تطوير الحديقة العامة',
    description: 'تجديد وتطوير الحديقة العامة مع إضافة ملاعب للأطفال',
    location: 'طرطوس',
    progress: 45,
    contractor: 'مؤسسة الخضراء',
    startDate: '2024-02-01',
    expectedEnd: '2024-08-15'
  }
])

const completedProjects = ref([
  {
    id: 6,
    name: 'إعادة إعمار المسجد الكبير',
    description: 'ترميم وإعادة بناء المسجد التاريخي',
    location: 'حلب',
    completedDate: '2024-01-10',
    rating: 0,
    hasRated: false,
    contractor: 'شركة التراث للبناء'
  },
  {
    id: 7,
    name: 'تأهيل الطريق الرئيسي',
    description: 'إعادة تعبيد وإنارة الطريق الرئيسي',
    location: 'دمشق',
    completedDate: '2023-12-20',
    rating: 4,
    hasRated: true,
    contractor: 'شركة الطرق الحديثة'
  },
  {
    id: 8,
    name: 'ترميم المكتبة العامة',
    description: 'إعادة تأهيل المكتبة وتجهيزها بالكتب والحاسوب',
    location: 'حمص',
    completedDate: '2023-11-15',
    rating: 5,
    hasRated: true,
    contractor: 'مؤسسة المعرفة'
  }
])

// Statistics computed properties
const stats = computed(() => [
  {
    title: 'المشاريع المقترحة',
    value: proposedProjects.value.length,
    icon: 'mdi-lightbulb-outline',
    color: 'info',
    description: 'مشاريع يمكنك التصويت عليها'
  },
  {
    title: 'المشاريع قيد العمل',
    value: activeProjects.value.length,
    icon: 'mdi-hammer-wrench',
    color: 'warning',
    description: 'مشاريع يجري العمل عليها حالياً'
  },
  {
    title: 'المشاريع المنتهية',
    value: completedProjects.value.length,
    icon: 'mdi-check-circle',
    color: 'success',
    description: 'مشاريع تم إنجازها'
  },
  {
    title: 'المشاريع المقيمة',
    value: completedProjects.value.filter(p => p.hasRated).length,
    icon: 'mdi-star',
    color: 'primary',
    description: 'مشاريع قمت بتقييمها'
  }
])

// Voting functionality
const voteForProject = (projectId: number) => {
  const project = proposedProjects.value.find(p => p.id === projectId)
  if (project && !project.hasVoted) {
    project.votes++
    project.hasVoted = true
  }
}

// Rating functionality
const rateProject = (projectId: number, rating: number) => {
  const project = completedProjects.value.find(p => p.id === projectId)
  if (project && !project.hasRated) {
    project.rating = rating
    project.hasRated = true
  }
}

// Get priority color
const getPriorityColor = (priority: string) => {
  const colors = {
    'high': 'error',
    'medium': 'warning',
    'low': 'success'
  }
  return colors[priority] || 'grey'
}

// Get priority text
const getPriorityText = (priority: string) => {
  const texts = {
    'high': 'عالية',
    'medium': 'متوسطة',
    'low': 'منخفضة'
  }
  return texts[priority] || 'غير محدد'
}

// Format date
const formatDate = (dateString: string) => {
  return new Date(dateString).toLocaleDateString('ar-SA')
}

// Calculate days since completion
const daysSinceCompletion = (dateString: string) => {
  const completedDate = new Date(dateString)
  const today = new Date()
  const diffTime = Math.abs(today.getTime() - completedDate.getTime())
  const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24))
  return diffDays
}
</script>

<template>
  <v-container>
    <!-- Header -->
    <v-row>
      <v-col cols="12">
        <div class="d-flex align-center mb-4">
          <div>
            <h1 class="text-h4">لوحة تحكم المواطن</h1>
            <p class="text-subtitle-1 text-medium-emphasis" v-if="user">
              {{ user.name }} - شارك في إعادة إعمار سوريا
            </p>
          </div>
          <v-spacer></v-spacer>
          <v-chip color="primary" variant="outlined" size="large">
            مواطن
          </v-chip>
        </div>
      </v-col>
    </v-row>
    
    <!-- Statistics Cards -->
    <v-row>
      <v-col v-for="(stat, i) in stats" :key="i" cols="12" sm="6" lg="3">
        <v-card
          class="mx-auto h-100 d-flex flex-column"
          variant="elevated"
          :color="stat.color"
          theme="dark"
          hover
        >
          <v-card-item>
            <template v-slot:prepend>
              <v-icon :icon="stat.icon" size="large"></v-icon>
            </template>
            <v-card-title class="text-wrap">{{ stat.title }}</v-card-title>
          </v-card-item>
          <v-card-text class="text-center flex-grow-1 d-flex flex-column justify-center">
            <div class="text-h2 mb-2">{{ stat.value }}</div>
            <div class="text-body-2 opacity-90">{{ stat.description }}</div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Proposed Projects Section -->
    <v-row class="mt-6">
      <v-col cols="12">
        <v-card variant="outlined">
          <v-card-title class="d-flex align-center">
            <v-icon icon="mdi-lightbulb-outline" class="ml-2"></v-icon>
            المشاريع المقترحة - صوت لأولوياتك
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col v-for="project in proposedProjects" :key="project.id" cols="12" md="6" lg="4">
                <v-card class="h-100" variant="outlined">
                  <v-card-item>
                    <v-card-title class="text-wrap">{{ project.name }}</v-card-title>
                    <v-card-subtitle>{{ project.location }}</v-card-subtitle>
                  </v-card-item>
                  <v-card-text>
                    <p class="text-body-2 mb-3">{{ project.description }}</p>
                    <div class="d-flex justify-space-between align-center mb-3">
                      <v-chip :color="getPriorityColor(project.priority)" size="small">
                        {{ getPriorityText(project.priority) }}
                      </v-chip>
                      <div class="d-flex align-center">
                        <v-icon icon="mdi-thumb-up" size="small" class="ml-1"></v-icon>
                        <span class="text-h6">{{ project.votes }}</span>
                      </div>
                    </div>
                  </v-card-text>
                  <v-card-actions>
                    <v-btn
                      :color="project.hasVoted ? 'success' : 'primary'"
                      :variant="project.hasVoted ? 'outlined' : 'elevated'"
                      :disabled="project.hasVoted"
                      @click="voteForProject(project.id)"
                      block
                    >
                      <v-icon :icon="project.hasVoted ? 'mdi-check' : 'mdi-thumb-up'" start></v-icon>
                      {{ project.hasVoted ? 'تم التصويت' : 'صوت للمشروع' }}
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Active Projects Section -->
    <v-row class="mt-6">
      <v-col cols="12">
        <v-card variant="outlined">
          <v-card-title class="d-flex align-center">
            <v-icon icon="mdi-hammer-wrench" class="ml-2"></v-icon>
            المشاريع قيد العمل
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col v-for="project in activeProjects" :key="project.id" cols="12" md="6">
                <v-card class="h-100" variant="outlined">
                  <v-card-item>
                    <v-card-title class="text-wrap">{{ project.name }}</v-card-title>
                    <v-card-subtitle>{{ project.location }} - {{ project.contractor }}</v-card-subtitle>
                  </v-card-item>
                  <v-card-text>
                    <p class="text-body-2 mb-3">{{ project.description }}</p>
                    <div class="mb-3">
                      <div class="d-flex justify-space-between align-center mb-2">
                        <span class="text-body-2">نسبة الإنجاز</span>
                        <span class="text-h6 text-primary">{{ project.progress }}%</span>
                      </div>
                      <v-progress-linear 
                        :model-value="project.progress" 
                        color="primary" 
                        height="8" 
                        rounded
                      ></v-progress-linear>
                    </div>
                    <div class="d-flex justify-space-between text-caption">
                      <span>بدء: {{ formatDate(project.startDate) }}</span>
                      <span>متوقع الانتهاء: {{ formatDate(project.expectedEnd) }}</span>
                    </div>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Completed Projects Section -->
    <v-row class="mt-6">
      <v-col cols="12">
        <v-card variant="outlined">
          <v-card-title class="d-flex align-center">
            <v-icon icon="mdi-check-circle" class="ml-2"></v-icon>
            المشاريع المنتهية - قيم جودة العمل
          </v-card-title>
          <v-card-text>
            <v-row>
              <v-col v-for="project in completedProjects" :key="project.id" cols="12" md="6" lg="4">
                <v-card class="h-100" variant="outlined">
                  <v-card-item>
                    <v-card-title class="text-wrap">{{ project.name }}</v-card-title>
                    <v-card-subtitle>{{ project.location }} - {{ project.contractor }}</v-card-subtitle>
                  </v-card-item>
                  <v-card-text>
                    <p class="text-body-2 mb-3">{{ project.description }}</p>
                    <div class="d-flex justify-space-between align-center mb-3">
                      <span class="text-caption">
                        انتهى منذ {{ daysSinceCompletion(project.completedDate) }} يوم
                      </span>
                      <v-chip color="success" size="small" variant="outlined">
                        مكتمل
                      </v-chip>
                    </div>
                    
                    <!-- Rating Section -->
                    <div class="text-center">
                      <p class="text-body-2 mb-2">{{ project.hasRated ? 'تقييمك:' : 'قيم جودة المشروع:' }}</p>
                      <v-rating
                        v-model="project.rating"
                        :readonly="project.hasRated"
                        :color="project.hasRated ? 'amber' : 'grey'"
                        active-color="amber"
                        size="large"
                        @update:model-value="(value) => rateProject(project.id, value)"
                      ></v-rating>
                      <p v-if="project.hasRated" class="text-caption mt-1 text-success">
                        شكراً لك على التقييم!
                      </p>
                    </div>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Summary Section -->
    <v-row class="mt-6">
      <v-col cols="12">
        <v-alert type="info" variant="tonal">
          <v-alert-title>مشاركتك مهمة!</v-alert-title>
          صوتك يساعد في تحديد أولويات إعادة الإعمار. تقييمك للمشاريع المنتهية يساعد في تحسين جودة العمل المستقبلي.
        </v-alert>
      </v-col>
    </v-row>
  </v-container>
</template>