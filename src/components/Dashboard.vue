<script setup lang="ts">
import { ref } from 'vue'

const items = ref([
  { title: 'المهام', icon: 'mdi-clipboard-list', value: 12, color: 'primary' },
  { title: 'المستخدمون', icon: 'mdi-account-group', value: 42, color: 'info' },
  { title: 'الرسائل', icon: 'mdi-email', value: 8, color: 'success' },
  { title: 'التنبيهات', icon: 'mdi-bell', value: 3, color: 'warning' }
])

const recentActivities = ref([
  { user: 'أحمد محمد', action: 'أضاف مستخدم جديد', time: 'منذ 5 دقائق', icon: 'mdi-account-plus', color: 'success' },
  { user: 'سارة أحمد', action: 'حذفت مهمة', time: 'منذ 10 دقائق', icon: 'mdi-delete', color: 'error' },
  { user: 'محمد علي', action: 'عدل التقرير الشهري', time: 'منذ 25 دقيقة', icon: 'mdi-file-edit', color: 'primary' },
  { user: 'نور محمد', action: 'أرسلت رسالة', time: 'منذ ساعة', icon: 'mdi-email-send', color: 'info' }
])
</script>

<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1 class="text-h4 mb-4">لوحة التحكم</h1>
      </v-col>
    </v-row>
    
    <v-row>
      <v-col v-for="(item, i) in items" :key="i" cols="12" sm="6" lg="3">
        <v-card
          class="mx-auto h-100 d-flex flex-column"
          variant="elevated"
          :color="item.color"
          theme="dark"
        >
          <v-card-item>
            <template v-slot:prepend>
              <v-icon :icon="item.icon" size="large"></v-icon>
            </template>
            <v-card-title>{{ item.title }}</v-card-title>
          </v-card-item>
          <v-card-text class="text-center">
            <div class="text-h2">{{ item.value }}</div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <v-row class="mt-6">
      <v-col cols="12" md="8">
        <v-card title="المهام الحالية" variant="outlined">
          <v-card-text>
            <v-progress-linear 
              model-value="70" 
              color="primary" 
              height="20" 
              striped
            >
              <template v-slot:default="{ value }">
                <strong>{{ Math.ceil(value) }}%</strong>
              </template>
            </v-progress-linear>
            
            <div class="d-flex justify-space-between mt-4">
              <div>
                <div class="text-h6">المهام المكتملة</div>
                <div class="text-subtitle-1">7 من 10</div>
              </div>
              <div>
                <div class="text-h6">المهام الجارية</div>
                <div class="text-subtitle-1">3 من 10</div>
              </div>
            </div>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-btn variant="text" color="primary">
              عرض كل المهام
              <v-icon icon="mdi-arrow-left" end></v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      
      <v-col cols="12" md="4">
        <v-card title="الأنشطة الأخيرة" variant="outlined">
          <v-card-text class="pb-0">
            <v-timeline density="compact" align="start">
              <v-timeline-item
                v-for="(activity, i) in recentActivities"
                :key="i"
                :dot-color="activity.color"
                :icon="activity.icon"
                size="small"
              >
                <div class="mb-4">
                  <div class="font-weight-bold">{{ activity.user }}</div>
                  <div>{{ activity.action }}</div>
                  <div class="text-caption">{{ activity.time }}</div>
                </div>
              </v-timeline-item>
            </v-timeline>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>