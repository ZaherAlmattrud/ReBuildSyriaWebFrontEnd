<script setup lang="ts">
import { ref, computed } from 'vue'
import Dashboard from '../components/Dashboard.vue'
import Settings from '../components/Settings.vue'
import LandingPage from '../components/LandingPage.vue'

const drawer = ref(true)
const rail = ref(false)
const currentTheme = ref('light')

const items = [
  // { title: 'الرئيسية', icon: 'mdi-home', route: '/' },
  { title: 'لوحة التحكم', icon: 'mdi-view-dashboard', route: '/dashboard' },
  { title: 'المشاريع', icon: 'mdi-crane', route: '/projects' },
  { title: 'التقارير', icon: 'mdi-chart-bar', route: '/reports' },
  { title: 'الإعدادات', icon: 'mdi-cog', route: '/settings' }
]

const currentRoute = ref('/dashboard')

function navigateTo(route) {
  currentRoute.value = route
}

const isDark = computed({
  get: () => currentTheme.value === 'dark',
  set: (value) => {
    currentTheme.value = value ? 'dark' : 'light'
  }
})
</script>

<template>
  <v-navigation-drawer
    v-model="drawer"
    :rail="rail"
    permanent
    @click="rail = false"
  >
    <v-list-item
      prepend-avatar="https://randomuser.me/api/portraits/men/85.jpg"
      title="محمد أحمد"
      subtitle="مدير النظام"
    >
      <template v-slot:append>
        <v-btn
          variant="text"
          icon="mdi-chevron-right"
          @click.stop="rail = !rail"
        ></v-btn>
      </template>
    </v-list-item>

    <v-divider></v-divider>

    <v-list density="compact" nav>
      <v-list-item
        v-for="(item, i) in items"
        :key="i"
        :value="item"
        :title="item.title"
        :prepend-icon="item.icon"
        :active="currentRoute === item.route"
        @click="navigateTo(item.route)"
        :class="{ 'v-list-item--active': currentRoute === item.route }"
      ></v-list-item>
    </v-list>
  </v-navigation-drawer>

  <v-app-bar elevation="1">
    <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
    <v-app-bar-title>منصة إعادة إعمار سوريا</v-app-bar-title>
    <v-spacer></v-spacer>
    <v-btn icon @click="isDark = !isDark">
      <v-icon>{{ isDark ? 'mdi-weather-sunny' : 'mdi-weather-night' }}</v-icon>
    </v-btn>
    <v-btn icon>
      <v-icon>mdi-bell</v-icon>
      <v-badge
        color="error"
        content="3"
        dot
      ></v-badge>
    </v-btn>
  </v-app-bar>

  <v-main>
    <transition name="slide-fade" mode="out-in">
      <keep-alive>
        <LandingPage v-if="currentRoute === '/'" />
        <Dashboard v-else-if="currentRoute === '/dashboard'" />
        <Settings v-else-if="currentRoute === '/settings'" />
        <v-container v-else>
          <v-row>
            <v-col cols="12">
              <v-sheet class="pa-4 rounded">
                <h1 class="text-h4 mb-4">قيد التطوير</h1>
                <p class="text-body-1">هذه الصفحة قيد التطوير.</p>
                <v-btn
                  color="primary"
                  prepend-icon="mdi-home"
                  class="mt-4"
                  @click="navigateTo('/')"
                >
                  العودة للرئيسية
                </v-btn>
              </v-sheet>
            </v-col>
          </v-row>
        </v-container>
      </keep-alive>
    </transition>
  </v-main>

  <v-footer app class="d-flex flex-column">
    <div class="text-center w-100">
      <span>&copy; {{ new Date().getFullYear() }} - منصة إعادة إعمار سوريا</span>
    </div>
  </v-footer>
</template>

<style scoped>
.v-list-item--active {
  background-color: rgba(var(--v-theme-primary), 0.1);
}
</style>