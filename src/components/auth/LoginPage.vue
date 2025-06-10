<script setup lang="ts">
import { ref } from 'vue'

const email = ref('')
const password = ref('')
const rememberMe = ref(false)
const loading = ref(false)
const errorMessage = ref('')

const emit = defineEmits(['login'])

// Mock user database with roles
const mockUsers = {
  'citizen@example.com': { role: 1, name: 'أحمد محمد', type: 'مواطن' },
  'gov@rebuildsyria.gov.sy': { role: 2, name: 'وزارة الإعمار', type: 'جهة حكومية' },
  'org@ngo.org': { role: 3, name: 'منظمة الإغاثة', type: 'منظمة' },
  'contractor@build.com': { role: 4, name: 'شركة البناء', type: 'مقاول' },
  'supervisor@eval.gov.sy': { role: 5, name: 'جهة التقييم', type: 'جهة تقييم و إشراف' },
  'media@news.sy': { role: 6, name: 'قناة الإخبارية', type: 'إعلام' }
}

const handleLogin = async () => {
  loading.value = true
  errorMessage.value = ''

  try {
    // Simulate API call delay
    await new Promise(resolve => setTimeout(resolve, 1000))

    // Check if user exists in mock database
    const user = mockUsers[email.value.toLowerCase()]
    
    if (!user) {
      errorMessage.value = 'البريد الإلكتروني غير مسجل في النظام'
      return
    }

    // Simulate password validation (in real app, this would be done on server)
    if (password.value.length < 6) {
      errorMessage.value = 'كلمة المرور غير صحيحة'
      return
    }

    // Store user info in localStorage (in real app, use proper token management)
    localStorage.setItem('user', JSON.stringify({
      email: email.value,
      name: user.name,
      role: user.role,
      type: user.type
    }))

    emit('login', user)
  } catch (error) {
    errorMessage.value = 'حدث خطأ أثناء تسجيل الدخول'
  } finally {
    loading.value = false
  }
}

const handleSocialLogin = (provider: string) => {
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
            <p class="text-medium-emphasis mt-2">
              سيتم تحديد نوع حسابك تلقائياً بناءً على بريدك الإلكتروني
            </p>
          </div>
          
          <v-form @submit.prevent="handleLogin">
            <v-text-field
              v-model="email"
              label="البريد الإلكتروني"
              type="email"
              required
              prepend-inner-icon="mdi-email"
              :error-messages="errorMessage && errorMessage.includes('البريد') ? [errorMessage] : []"
            ></v-text-field>

            <v-text-field
              v-model="password"
              label="كلمة المرور"
              type="password"
              required
              prepend-inner-icon="mdi-lock"
              :error-messages="errorMessage && errorMessage.includes('كلمة المرور') ? [errorMessage] : []"
            ></v-text-field>

            <v-alert
              v-if="errorMessage && !errorMessage.includes('البريد') && !errorMessage.includes('كلمة المرور')"
              type="error"
              class="mb-4"
            >
              {{ errorMessage }}
            </v-alert>

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
              :loading="loading"
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

            <!-- Demo accounts info -->
            <v-expansion-panels class="mt-4">
              <v-expansion-panel>
                <v-expansion-panel-title>
                  <v-icon class="ml-2">mdi-information</v-icon>
                  حسابات تجريبية للاختبار
                </v-expansion-panel-title>
                <v-expansion-panel-text>
                  <v-list density="compact">
                    <v-list-item>
                      <v-list-item-title>مواطن: citizen@example.com</v-list-item-title>
                    </v-list-item>
                    <v-list-item>
                      <v-list-item-title>جهة حكومية: gov@rebuildsyria.gov.sy</v-list-item-title>
                    </v-list-item>
                    <v-list-item>
                      <v-list-item-title>منظمة: org@ngo.org</v-list-item-title>
                    </v-list-item>
                    <v-list-item>
                      <v-list-item-title>مقاول: contractor@build.com</v-list-item-title>
                    </v-list-item>
                    <v-list-item>
                      <v-list-item-title>جهة تقييم: supervisor@eval.gov.sy</v-list-item-title>
                    </v-list-item>
                    <v-list-item>
                      <v-list-item-title>إعلام: media@news.sy</v-list-item-title>
                    </v-list-item>
                    <v-list-item class="text-caption">
                      كلمة المرور: أي كلمة مرور أكثر من 6 أحرف
                    </v-list-item>
                  </v-list>
                </v-expansion-panel-text>
              </v-expansion-panel>
            </v-expansion-panels>
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>