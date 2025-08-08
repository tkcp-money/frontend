<script setup lang="ts">
import * as z from 'zod'
import type { FormSubmitEvent } from '@nuxt/ui'

// Устанавливаем layout для этой страницы
definePageMeta({
  layout: 'auth'
})

const toast = useToast()

// Локальный стейт для переключения между формами
const isLogin = ref(true)

// Поля для формы авторизации
const loginFields = [{
  name: 'email',
  type: 'text' as const,
  label: 'Email',
  placeholder: 'Enter your email',
  required: true
}, {
  name: 'password',
  label: 'Password',
  type: 'password' as const,
  placeholder: 'Enter your password'
}, {
  name: 'remember',
  label: 'Remember me',
  type: 'checkbox' as const
}]

// Поля для формы регистрации
const registerFields = [{
  name: 'name',
  type: 'text' as const,
  label: 'Full Name',
  placeholder: 'Enter your full name',
  required: true
}, {
  name: 'email',
  type: 'text' as const,
  label: 'Email',
  placeholder: 'Enter your email',
  required: true
}, {
  name: 'password',
  label: 'Password',
  type: 'password' as const,
  placeholder: 'Enter your password',
  required: true
}, {
  name: 'confirmPassword',
  label: 'Confirm Password',
  type: 'password' as const,
  placeholder: 'Confirm your password',
  required: true
}]

const providers = [{
  label: 'Google',
  icon: 'i-simple-icons-google',
  onClick: () => {
    toast.add({ title: 'Google', description: isLogin.value ? 'Login with Google' : 'Register with Google' })
  }
}, {
  label: 'GitHub',
  icon: 'i-simple-icons-github',
  onClick: () => {
    toast.add({ title: 'GitHub', description: isLogin.value ? 'Login with GitHub' : 'Register with GitHub' })
  }
}]

// Схема валидации для авторизации
const loginSchema = z.object({
  email: z.string().email('Invalid email'),
  password: z.string().min(8, 'Must be at least 8 characters')
})

// Схема валидации для регистрации
const registerSchema = z.object({
  name: z.string().min(2, 'Name must be at least 2 characters'),
  email: z.string().email('Invalid email'),
  password: z.string().min(8, 'Must be at least 8 characters'),
  confirmPassword: z.string()
}).refine((data) => data.password === data.confirmPassword, {
  message: "Passwords don't match",
  path: ["confirmPassword"],
})

type LoginSchema = z.output<typeof loginSchema>
type RegisterSchema = z.output<typeof registerSchema>

function onLoginSubmit(payload: FormSubmitEvent<LoginSchema>) {
  console.log('Login submitted', payload)
  toast.add({ title: 'Success', description: 'Login successful' })
}

function onRegisterSubmit(payload: FormSubmitEvent<RegisterSchema>) {
  console.log('Register submitted', payload)
  toast.add({ title: 'Success', description: 'Registration successful' })
}

function toggleForm() {
  isLogin.value = !isLogin.value
}
</script>

<template>
  <!-- Форма авторизации -->
  <UAuthForm
      v-if="isLogin"
      :schema="loginSchema"
      title="Welcome Back"
      description="Sign in to your account to continue"
      icon="i-lucide-user"
      :fields="loginFields"
      :providers="providers"
      @submit="onLoginSubmit"
  >
    <template #footer>
      <div class="text-center mt-4">
        <p class="text-sm text-gray-600">
          Don't have an account?
          <UButton
              variant="link"
              color="primary"
              @click="toggleForm"
              class="p-0 h-auto"
          >
            Sign up
          </UButton>
        </p>
      </div>
    </template>
  </UAuthForm>

  <!-- Форма регистрации -->
  <UAuthForm
      v-else
      :schema="registerSchema"
      title="Create Account"
      description="Join us and start managing your finances"
      icon="i-lucide-user-plus"
      :fields="registerFields"
      :providers="providers"
      @submit="onRegisterSubmit"
  >
    <template #footer>
      <div class="text-center mt-4">
        <p class="text-sm text-gray-600">
          Already have an account?
          <UButton
              variant="link"
              color="primary"
              @click="toggleForm"
              class="p-0 h-auto"
          >
            Sign in
          </UButton>
        </p>
      </div>
    </template>
  </UAuthForm>
</template>

