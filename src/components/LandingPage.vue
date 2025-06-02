<script setup lang="ts">
import { defineEmits } from 'vue'

const emit = defineEmits(['login'])

const features = [
  {
    title: 'المواطنون',
    icon: 'mdi-account-group',
    description: 'الإبلاغ عن أضرار البنية التحتية في مناطقهم وتتبع عملية إعادة الإعمار',
    color: 'primary'
  },
  {
    title: 'الحكومة',
    icon: 'mdi-office-building',
    description: 'تحديد الأولويات وإدارة مشاريع إعادة الإعمار بشكل فعال',
    color: 'success'
  },
  {
    title: 'المنظمات',
    icon: 'mdi-handshake',
    description: 'التنسيق مع الجهات المعنية وتقديم المساعدة في عملية إعادة الإعمار',
    color: 'info'
  },
  {
    title: 'الإعلام',
    icon: 'mdi-newspaper',
    description: 'متابعة تطورات عملية إعادة الإعمار ونشر التقارير والتحديثات',
    color: 'warning'
  }
]

const statistics = [
  {
    value: '1000+',
    label: 'مشروع إعادة إعمار',
    icon: 'mdi-crane'
  },
  {
    value: '50+',
    label: 'منظمة مشاركة',
    icon: 'mdi-account-group'
  },
  {
    value: '14',
    label: 'محافظة مشمولة',
    icon: 'mdi-map'
  },
  {
    value: '24/7',
    label: 'متابعة مستمرة',
    icon: 'mdi-clock'
  }
]
</script>

<template>
  <div>
    <!-- Hero Section -->
    <div class="hero-section">
      <v-img
        src="/src/assets/LandingPage.jpg"
        height="400"
        cover
        class="hero-image"
      >
        <div class="hero-overlay">
          <div class="text-center">
            <h1 class="text-h2 font-weight-bold mb-4">منصة إعادة إعمار سوريا</h1>
            <h2 class="text-h5 mb-4">معاً نحو مستقبل أفضل</h2>
            <v-btn
              size="x-large"
              color="primary"
              class="mt-4"
              @click="emit('login')"
            >
              ابدأ الآن
              <v-icon end>mdi-arrow-left</v-icon>
            </v-btn>
          </div>
        </div>
      </v-img>
    </div>

    <!-- Features Section -->
    <v-container class="py-12">
      <h2 class="text-h4 text-center mb-8">كيف تعمل المنصة؟</h2>
      <v-row>
        <v-col
          v-for="(feature, i) in features"
          :key="i"
          cols="12"
          md="6"
          lg="3"
        >
          <v-card
            class="mx-auto h-100"
            elevation="2"
          >
            <v-card-item>
              <template v-slot:prepend>
                <v-icon
                  :color="feature.color"
                  size="x-large"
                  :icon="feature.icon"
                ></v-icon>
              </template>
              <v-card-title>{{ feature.title }}</v-card-title>
            </v-card-item>
            <v-card-text class="text-body-1">
              {{ feature.description }}
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>

    <!-- Statistics Section -->
    <v-sheet color="grey-lighten-3" class="py-12">
      <v-container>
        <v-row>
          <v-col
            v-for="(stat, i) in statistics"
            :key="i"
            cols="6"
            md="3"
            class="text-center"
          >
            <v-icon
              :icon="stat.icon"
              color="primary"
              size="x-large"
              class="mb-4"
            ></v-icon>
            <div class="text-h4 font-weight-bold mb-2">{{ stat.value }}</div>
            <div class="text-body-1">{{ stat.label }}</div>
          </v-col>
        </v-row>
      </v-container>
    </v-sheet>

    <!-- Call to Action -->
    <v-container class="py-12 text-center">
      <h2 class="text-h4 mb-6">شارك في إعادة إعمار سوريا</h2>
      <p class="text-body-1 mb-6">
        انضم إلينا اليوم وكن جزءاً من عملية إعادة الإعمار. معاً نستطيع بناء مستقبل أفضل.
      </p>
      <v-btn
        color="primary"
        size="x-large"
        class="mx-2"
        @click="emit('login')"
      >
        سجل كمواطن
        <v-icon end>mdi-account</v-icon>
      </v-btn>
      <v-btn
        color="secondary"
        size="x-large"
        variant="outlined"
        class="mx-2"
        @click="emit('login')"
      >
        سجل كمنظمة
        <v-icon end>mdi-domain</v-icon>
      </v-btn>
    </v-container>
  </div>
</template>

<style scoped>
.hero-section {
  position: relative;
}

.hero-image {
  width: 100%;
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
}
</style>