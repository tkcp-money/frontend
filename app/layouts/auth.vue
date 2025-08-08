<script setup lang="ts">
// Layout для страниц авторизации
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-slate-50 via-white to-emerald-50 dark:from-slate-900 dark:via-slate-800 dark:to-emerald-900/20">
    <!-- Фоновые элементы -->
    <div class="absolute inset-0 overflow-hidden">
      <!-- Светлая тема -->
      <div class="absolute -top-40 -right-40 w-80 h-80 bg-blue-200 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob dark:hidden"></div>
      <div class="absolute -bottom-40 -left-40 w-80 h-80 bg-purple-200 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob animation-delay-2000 dark:hidden"></div>
      <div class="absolute top-40 left-40 w-80 h-80 bg-pink-200 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob animation-delay-4000 dark:hidden"></div>
      
      <!-- Темная тема -->
      <div class="absolute -top-40 -right-40 w-80 h-80 bg-emerald-500/20 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob hidden dark:block"></div>
      <div class="absolute -bottom-40 -left-40 w-80 h-80 bg-slate-600/20 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob animation-delay-2000 hidden dark:block"></div>
      <div class="absolute top-40 left-40 w-80 h-80 bg-teal-500/20 rounded-full mix-blend-multiply filter blur-xl opacity-70 animate-blob animation-delay-4000 hidden dark:block"></div>
    </div>

    <!-- Основной контент -->
    <div class="relative z-10 min-h-screen flex items-center justify-center p-4">
      <!-- Логотип и заголовок -->
      <div class="absolute top-8 left-8">
        <div class="flex items-center space-x-2">
          <div class="w-8 h-8 bg-gradient-to-r from-emerald-600 to-teal-600 dark:from-emerald-500 dark:to-teal-500 rounded-lg flex items-center justify-center">
            <UIcon name="i-lucide-wallet" class="w-5 h-5 text-white" />
          </div>
          <span class="text-xl font-bold text-slate-800 dark:text-slate-100">TKCP</span>
        </div>
      </div>

      <!-- Контейнер для формы -->
      <div class="w-full max-w-md">
        <UPageCard class="backdrop-blur-sm bg-white/80 dark:bg-slate-800/80 border border-white/20 dark:border-slate-700/50 shadow-xl dark:shadow-2xl">
          <slot />
        </UPageCard>
      </div>

      <!-- Футер -->
      <div class="absolute bottom-8 left-1/2 transform -translate-x-1/2">
        <p class="text-sm text-slate-500 dark:text-slate-400 text-center">
          © 2024 TKCP. All rights reserved.
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
@keyframes blob {
  0% {
    transform: translate(0px, 0px) scale(1);
  }
  33% {
    transform: translate(30px, -50px) scale(1.1);
  }
  66% {
    transform: translate(-20px, 20px) scale(0.9);
  }
  100% {
    transform: translate(0px, 0px) scale(1);
  }
}

.animate-blob {
  animation: blob 7s infinite;
}

.animation-delay-2000 {
  animation-delay: 2s;
}

.animation-delay-4000 {
  animation-delay: 4s;
}

/* Дополнительные стили для форм */
:deep(.u-auth-form) {
  background: transparent;
}

:deep(.u-auth-form .u-card) {
  background: transparent;
  box-shadow: none;
  border: none;
}

:deep(.u-auth-form .u-card-body) {
  padding: 0;
}

/* Стили для темной темы */
:deep(.dark .u-auth-form) {
  background: transparent;
}

:deep(.dark .u-auth-form .u-card) {
  background: transparent;
  box-shadow: none;
  border: none;
}

:deep(.dark .u-auth-form .u-card-body) {
  padding: 0;
}
</style>