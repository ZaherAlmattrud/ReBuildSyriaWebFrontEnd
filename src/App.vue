<script setup lang="ts">
import { ref, onMounted } from 'vue'
import MainLayout from './layouts/MainLayout.vue'
import LandingPage from './components/LandingPage.vue'
import LoginPage from './components/auth/LoginPage.vue'
import RegisterPage from './components/auth/RegisterPage.vue'
import ForgotPasswordPage from './components/auth/ForgotPasswordPage.vue'

const isAuthenticated = ref(false)
const currentRoute = ref('/')
const currentUser = ref(null)

// Check if user is already logged in on app start
onMounted(() => {
  const savedUser = localStorage.getItem('user')
  if (savedUser) {
    currentUser.value = JSON.parse(savedUser)
    isAuthenticated.value = true
  }
})

const handleLogin = (user) => {
  isAuthenticated.value = true
  currentUser.value = user
}

const handleLogout = () => {
  localStorage.removeItem('user')
  isAuthenticated.value = false
  currentUser.value = null
  currentRoute.value = '/'
}

const navigateTo = (route: string) => {
  currentRoute.value = route
}
</script>

<template>
  <v-app>
    <template v-if="isAuthenticated">
      <MainLayout :user="currentUser" @logout="handleLogout" />
    </template>
    <template v-else>
      <v-app-bar elevation="1">
        <v-app-bar-title>ReBuildSyria</v-app-bar-title>
        <v-spacer></v-spacer>
        <v-btn color="primary" class="ml-2" prepend-icon="mdi-login" @click="navigateTo('/login')">
          تسجيل الدخول
        </v-btn>
        <v-btn color="secondary" variant="outlined" prepend-icon="mdi-account-plus" @click="navigateTo('/register')">
          إنشاء حساب
        </v-btn>
      </v-app-bar>
      <v-main>
        <template v-if="currentRoute === '/'">
          <LandingPage @login="navigateTo('/login')" />
        </template>
        <template v-else-if="currentRoute === '/login'">
          <LoginPage @login="handleLogin" />
        </template>
        <template v-else-if="currentRoute === '/register'">
          <RegisterPage />
        </template>
        <template v-else-if="currentRoute === '/forgot-password'">
          <ForgotPasswordPage />
        </template>
      </v-main>
      <v-footer app class="d-flex flex-column">
        <div class="text-center w-100">
          <span>&copy; {{ new Date().getFullYear() }} - ReBuildSyria</span>
        </div>
      </v-footer>
    </template>
  </v-app>
</template>