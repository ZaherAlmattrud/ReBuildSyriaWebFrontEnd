<script setup lang="ts">
import { ref } from 'vue'

const email = ref('')
const password = ref('')
const rememberMe = ref(false)

const emit = defineEmits(['login'])

const handleLogin = () => {
  // TODO: Implement login logic
  emit('login')
}

const handleSocialLogin = (provider: string) => {
  // TODO: Implement social login
  console.log(`Login with ${provider}`)
}
</script>

<template>
  <v-container class="fill-height">
    <v-row justify="center">
      <v-col cols="12" sm="8" md="6" lg="4">
        <v-card class="elevation-12 pa-6">
          <div class="text-center mb-6">
            <h1 class="text-h4 font-weight-bold">تسجيل الدخول</h1>
          </div>
          
          <v-form @submit.prevent="handleLogin">
            <v-text-field
              v-model="email"
              label="البريد الإلكتروني"
              type="email"
              required
              prepend-inner-icon="mdi-email"
            ></v-text-field>

            <v-text-field
              v-model="password"
              label="كلمة المرور"
              type="password"
              required
              prepend-inner-icon="mdi-lock"
            ></v-text-field>

            <div class="d-flex justify-space-between align-center mb-6">
              <v-checkbox
                v-model="rememberMe"
                label="تذكرني"
                hide-details
              ></v-checkbox>
              <v-btn
                variant="text"
                color="primary"
                to="/forgot-password"
                class="text-none"
              >
                نسيت كلمة المرور؟
              </v-btn>
            </div>

            <v-btn
              color="primary"
              block
              size="large"
              type="submit"
              class="mb-4"
            >
              تسجيل الدخول
            </v-btn>

            <div class="text-center mb-4">
              <span class="text-medium-emphasis">أو سجل الدخول باستخدام</span>
            </div>

            <div class="d-flex justify-space-between mb-6">
              <v-btn
                color="#DB4437"
                variant="flat"
                width="30%"
                @click="handleSocialLogin('google')"
              >
                <v-icon>mdi-google</v-icon>
              </v-btn>
              
              <v-btn
                color="#3B5998"
                variant="flat"
                width="30%"
                @click="handleSocialLogin('facebook')"
              >
                <v-icon>mdi-facebook</v-icon>
              </v-btn>
              
              <v-btn
                color="#000000"
                variant="flat"
                width="30%"
                @click="handleSocialLogin('github')"
              >
                <v-icon>mdi-github</v-icon>
              </v-btn>
            </div>

            <div class="text-center">
              <span class="text-medium-emphasis">ليس لديك حساب؟</span>
              <v-btn
                variant="text"
                color="primary"
                to="/register"
                class="text-none"
              >
                إنشاء حساب
              </v-btn>
            </div>
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>