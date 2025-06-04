<script setup lang="ts">
import { ref } from 'vue'

const settings = ref({
  notifications: true,
  darkMode: false,
  language: 'ar',
  emailNotifications: true,
  privateProfile: false,
  autoSave: true,
  theme: 'light'  // أضفت هذه السطر
})

const languages = [
  { title: 'العربية', value: 'ar' },
  { title: 'English', value: 'en' }
]

const themes = [
  { title: 'فاتح', value: 'light' },
  { title: 'داكن', value: 'dark' },
  { title: 'تلقائي', value: 'auto' }
]
</script>

<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <h1 class="text-h4 mb-4">الإعدادات</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" md="6">
        <v-card class="mb-4">
          <v-card-title>
            <v-icon icon="mdi-account-cog" class="ml-2"></v-icon>
            إعدادات الحساب
          </v-card-title>
          <v-card-text>
            <v-list>
              <v-list-item>
                <v-list-item-title>الوضع الداكن</v-list-item-title>
                <template v-slot:append>
                  <v-switch v-model="settings.darkMode" color="primary" hide-details></v-switch>
                </template>
              </v-list-item>

              <v-list-item>
                <v-list-item-title>اللغة</v-list-item-title>
                <template v-slot:append>
                  <v-select v-model="settings.language" :items="languages" item-title="title" item-value="value"
                    hide-details variant="plain" density="compact" style="max-width: 150px"></v-select>
                </template>
              </v-list-item>

              <v-list-item>
                <v-list-item-title>السمة</v-list-item-title>
                <template v-slot:append>
                  <v-select v-model="settings.theme" :items="themes" item-title="title" item-value="value" hide-details
                    variant="plain" density="compact" style="max-width: 150px"></v-select>
                </template>
              </v-list-item>

              <v-list-item>
                <v-list-item-title>ملف تعريف خاص</v-list-item-title>
                <template v-slot:append>
                  <v-switch v-model="settings.privateProfile" color="primary" hide-details></v-switch>
                </template>
              </v-list-item>
            </v-list>
          </v-card-text>
        </v-card>
      </v-col>

      <v-col cols="12" md="6">
        <v-card>
          <v-card-title>
            <v-icon icon="mdi-bell-cog" class="ml-2"></v-icon>
            إعدادات الإشعارات
          </v-card-title>
          <v-card-text>
            <v-list>
              <v-list-item>
                <v-list-item-title>تفعيل الإشعارات</v-list-item-title>
                <template v-slot:append>
                  <v-switch v-model="settings.notifications" color="primary" hide-details></v-switch>
                </template>
              </v-list-item>

              <v-list-item>
                <v-list-item-title>إشعارات البريد الإلكتروني</v-list-item-title>
                <template v-slot:append>
                  <v-switch v-model="settings.emailNotifications" color="primary" hide-details></v-switch>
                </template>
              </v-list-item>

              <v-list-item>
                <v-list-item-title>حفظ تلقائي</v-list-item-title>
                <template v-slot:append>
                  <v-switch v-model="settings.autoSave" color="primary" hide-details></v-switch>
                </template>
              </v-list-item>
            </v-list>
          </v-card-text>
        </v-card>

        <v-card class="mt-4">
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" variant="elevated">
              حفظ الإعدادات
            </v-btn>
            <v-btn color="error" variant="text">
              إعادة ضبط
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>