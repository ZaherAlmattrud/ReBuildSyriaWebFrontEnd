<script setup lang="ts">
import { ref } from 'vue'
import MainLayout from './layouts/MainLayout.vue'
import LandingPage from './components/LandingPage.vue'
import DashboardPage from './components/Dashboard.vue'

const isAuthenticated = ref(false)
</script>

<template>
  <v-app>
    <template v-if="isAuthenticated">
      <MainLayout />
    </template>
    <template v-else>
      <v-app-bar elevation="1">
        <v-app-bar-title>منصة إعادة إعمار سوريا</v-app-bar-title>
        <v-spacer></v-spacer>
        <v-btn color="primary" class="ml-2" prepend-icon="mdi-login" @click="isAuthenticated = true">
          تسجيل الدخول
        </v-btn>
        <v-btn color="secondary" variant="outlined" prepend-icon="mdi-account-plus">
          إنشاء حساب
        </v-btn>
      </v-app-bar>
      <v-main>
        <LandingPage @login="isAuthenticated = true" />
      </v-main>
      <v-footer app class="d-flex flex-column">
        <div class="text-center w-100">
          <span>&copy; {{ new Date().getFullYear() }} - منصة إعادة إعمار سوريا</span>
        </div>
      </v-footer>
    </template>
  </v-app>
</template>