<script setup lang="ts">
import { ref } from 'vue'

const email = ref('')
const submitted = ref(false)

const handleSubmit = () => {
  // TODO: Implement password reset logic
  submitted.value = true
}
</script>

<template>
  <v-container class="fill-height">
    <v-row justify="center">
      <v-col cols="12" sm="8" md="6" lg="4">
        <v-card class="elevation-12 pa-6">
          <div class="text-center mb-6">
            <h1 class="text-h4 font-weight-bold">استعادة كلمة المرور</h1>
          </div>
          
          <template v-if="!submitted">
            <p class="text-medium-emphasis mb-6">
              أدخل بريدك الإلكتروني وسنرسل لك رابطاً لإعادة تعيين كلمة المرور
            </p>
            
            <v-form @submit.prevent="handleSubmit">
              <v-text-field
                v-model="email"
                label="البريد الإلكتروني"
                type="email"
                required
                prepend-inner-icon="mdi-email"
                class="mb-6"
              ></v-text-field>

              <v-btn
                color="primary"
                block
                size="large"
                type="submit"
                class="mb-4"
              >
                إرسال رابط الاستعادة
              </v-btn>
            </v-form>
          </template>

          <template v-else>
            <v-alert
              type="success"
              text="تم إرسال رابط استعادة كلمة المرور إلى بريدك الإلكتروني"
              class="mb-6"
            ></v-alert>
          </template>

          <div class="text-center">
            <v-btn
              variant="text"
              color="primary"
              to="/login"
              class="text-none"
            >
              العودة إلى تسجيل الدخول
            </v-btn>
          </div>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>