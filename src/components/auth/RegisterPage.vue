<script setup lang="ts">
import { ref } from 'vue'

const form = ref({
  name: '',
  email: '',
  password: '',
  confirmPassword: '',
  agreeToTerms: false
})

const handleRegister = () => {
  // TODO: Implement registration logic
  console.log('Register', form.value)
}

const handleSocialRegister = (provider: string) => {
  // TODO: Implement social registration
  console.log(`Register with ${provider}`)
}
</script>

<template>
  <v-container class="fill-height">
    <v-row justify="center">
      <v-col cols="12" sm="8" md="6" lg="4">
        <v-card class="elevation-12 pa-6">
          <div class="text-center mb-6">
            <h1 class="text-h4 font-weight-bold">إنشاء حساب جديد</h1>
          </div>
          
          <v-form @submit.prevent="handleRegister">
            <v-text-field
              v-model="form.name"
              label="الاسم الكامل"
              required
              prepend-inner-icon="mdi-account"
            ></v-text-field>

            <v-text-field
              v-model="form.email"
              label="البريد الإلكتروني"
              type="email"
              required
              prepend-inner-icon="mdi-email"
            ></v-text-field>

            <v-text-field
              v-model="form.password"
              label="كلمة المرور"
              type="password"
              required
              prepend-inner-icon="mdi-lock"
            ></v-text-field>

            <v-text-field
              v-model="form.confirmPassword"
              label="تأكيد كلمة المرور"
              type="password"
              required
              prepend-inner-icon="mdi-lock-check"
            ></v-text-field>

            <v-checkbox
              v-model="form.agreeToTerms"
              label="أوافق على الشروط والأحكام"
              required
              class="mb-6"
            ></v-checkbox>

            <v-btn
              color="primary"
              block
              size="large"
              type="submit"
              class="mb-4"
            >
              إنشاء حساب
            </v-btn>

            <div class="text-center mb-4">
              <span class="text-medium-emphasis">أو سجل باستخدام</span>
            </div>

            <div class="d-flex justify-space-between mb-6">
              <v-btn
                color="#DB4437"
                variant="flat"
                width="30%"
                @click="handleSocialRegister('google')"
              >
                <v-icon>mdi-google</v-icon>
              </v-btn>
              
              <v-btn
                color="#3B5998"
                variant="flat"
                width="30%"
                @click="handleSocialRegister('facebook')"
              >
                <v-icon>mdi-facebook</v-icon>
              </v-btn>
              
              <v-btn
                color="#000000"
                variant="flat"
                width="30%"
                @click="handleSocialRegister('github')"
              >
                <v-icon>mdi-github</v-icon>
              </v-btn>
            </div>

            <div class="text-center">
              <span class="text-medium-emphasis">لديك حساب بالفعل؟</span>
              <v-btn
                variant="text"
                color="primary"
                to="/login"
                class="text-none"
              >
                تسجيل الدخول
              </v-btn>
            </div>
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>