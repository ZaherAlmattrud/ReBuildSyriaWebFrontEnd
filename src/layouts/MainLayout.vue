<script setup lang="ts">
import { ref, computed, defineProps, defineEmits } from 'vue'
import Dashboard from '../components/Dashboard.vue'
import Settings from '../components/Settings.vue'
import Projects from '../components/Projects.vue'

const props = defineProps(['user'])
const emit = defineEmits(['logout'])

const drawer = ref(true)
const rail = ref(false)
const currentTheme = ref('light')

// Navigation items based on user role
const getNavigationItems = () => {
  if (!props.user) return []
  
  // For citizens (role 1), only show Dashboard and Settings
  if (props.user.role === 1) {
    return [
      { title: 'لوحة التحكم', icon: 'mdi-view-dashboard', route: '/dashboard' },
      { title: 'الإعدادات', icon: 'mdi-cog', route: '/settings' }
    ]
  }
  
  // For other roles, show all items
  return [
    { title: 'لوحة التحكم', icon: 'mdi-view-dashboard', route: '/dashboard' },
    { title: 'المشاريع', icon: 'mdi-crane', route: '/projects' },
    { title: 'التقارير', icon: 'mdi-chart-bar', route: '/reports' },
    { title: 'الإعدادات', icon: 'mdi-cog', route: '/settings' }
  ]
}

const items = computed(() => getNavigationItems())

const currentRoute = ref('/dashboard') // Start with dashboard instead of home

function navigateTo(route) {
  currentRoute.value = route
}

const isDark = computed({
  get: () => currentTheme.value === 'dark',
  set: (value) => {
    currentTheme.value = value ? 'dark' : 'light'
  }
})

const getUserAvatar = () => {
  // Generate avatar based on user role
  const avatars = {
    1: 'https://randomuser.me/api/portraits/men/32.jpg', // مواطن
    2: 'https://randomuser.me/api/portraits/men/85.jpg', // جهة حكومية
    3: 'https://randomuser.me/api/portraits/women/44.jpg', // منظمة
    4: 'https://randomuser.me/api/portraits/men/76.jpg', // مقاول
    5: 'https://randomuser.me/api/portraits/women/68.jpg', // جهة تقييم
    6: 'https://randomuser.me/api/portraits/men/54.jpg' // إعلام
  }
  return avatars[props.user?.role] || avatars[1]
}

const handleLogout = () => {
  emit('logout')
}
</script>

<template>
  <v-navigation-drawer
    v-model="drawer"
    :rail="rail"
    permanent
    @click="rail = false"
  >
    <v-list-item
      :prepend-avatar="getUserAvatar()"
      :title="user?.name || 'مستخدم'"
      :subtitle="user?.type || 'غير محدد'"
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
    <v-app-bar-title>ReBuildSyria</v-app-bar-title>
    <v-spacer></v-spacer>
    
    <!-- User role indicator -->
    <v-chip
      v-if="user"
      :color="user.role === 2 ? 'success' : 'primary'"
      variant="outlined"
      size="small"
      class="ml-2"
    >
      {{ user.type }}
    </v-chip>
    
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
    
    <!-- Logout button -->
    <v-btn icon @click="handleLogout">
      <v-icon>mdi-logout</v-icon>
    </v-btn>
  </v-app-bar>

  <v-main>
    <transition name="slide-fade" mode="out-in">
      <keep-alive>
        <Dashboard v-if="currentRoute === '/dashboard'" :user="user" />
        <Projects v-else-if="currentRoute === '/projects'" :user="user" />
        <Settings v-else-if="currentRoute === '/settings'" />
        <v-container v-else>
          <v-row>
            <v-col cols="12">
              <v-sheet class="pa-4 rounded">
                <h1 class="text-h4 mb-4">قيد التطوير</h1>
                <p class="text-body-1">هذه الصفحة قيد التطوير.</p>
                <v-btn
                  color="primary"
                  prepend-icon="mdi-view-dashboard"
                  class="mt-4"
                  @click="navigateTo('/dashboard')"
                >
                  العودة للوحة التحكم
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
      <span>&copy; {{ new Date().getFullYear() }} - ReBuildSyria</span>
    </div>
  </v-footer>
</template>

<style scoped>
.v-list-item--active {
  background-color: rgba(var(--v-theme-primary), 0.1);
}
</style>