<script setup lang="ts">
import AppNavbar from "~/components/AppNavbar.vue";

// Статические данные для демонстрации
const currentPlan = ref({
  name: 'Pro',
  price: 29,
  currency: 'USD',
  period: 'month',
  status: 'active',
  nextBilling: '2024-02-15',
  features: [
    { name: 'Unlimited transactions', included: true },
    { name: 'Advanced analytics', included: true },
    { name: 'Export to Excel/PDF', included: true },
    { name: 'Multi-currency support', included: true },
    { name: 'Investment tracking', included: true },
    { name: 'Goal setting', included: true },
    { name: 'Team collaboration', included: false },
    { name: 'Priority support', included: false },
    { name: 'Custom reports', included: false },
    { name: 'API access', included: false }
  ]
})

const billingHistory = ref([
  {
    id: 'inv_001',
    date: '2024-01-15',
    amount: 29,
    currency: 'USD',
    status: 'paid',
    description: 'Pro Plan - Monthly'
  },
  {
    id: 'inv_002',
    date: '2023-12-15',
    amount: 29,
    currency: 'USD',
    status: 'paid',
    description: 'Pro Plan - Monthly'
  },
  {
    id: 'inv_003',
    date: '2023-11-15',
    amount: 29,
    currency: 'USD',
    status: 'paid',
    description: 'Pro Plan - Monthly'
  }
])

// Методы для управления подпиской
const upgradePlan = () => {
  // Здесь будет логика для апгрейда
  console.log('Upgrading to Premium plan')
}

const cancelSubscription = () => {
  // Здесь будет логика для отмены подписки
  console.log('Canceling subscription')
}

const downloadInvoice = (invoiceId: string) => {
  // Здесь будет логика для скачивания инвойса
  console.log('Downloading invoice:', invoiceId)
}

// Вычисляемые свойства
const includedFeatures = computed(() => 
  currentPlan.value.features.filter(f => f.included)
)

const excludedFeatures = computed(() => 
  currentPlan.value.features.filter(f => !f.included)
)

const nextBillingDate = computed(() => {
  return new Date(currentPlan.value.nextBilling).toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
})
</script>

<template>
  <UDashboardPanel id="subscription" :ui="{ body: 'sm:pt-1' }" >
    <template #body>
      <!-- Текущий план -->
      <UCard class="mb-6">
        <template #header>
          <div class="flex items-center justify-between">
            <div>
              <h3 class="text-lg font-semibold">Current Plan</h3>
              <p class="text-sm text-gray-600 dark:text-gray-400">Manage your subscription and billing</p>
            </div>
            <UBadge color="success" variant="soft">
              Active
            </UBadge>
          </div>
        </template>

        <div class="flex flex-col md:flex-row gap-6">
          <!-- Информация о плане -->
          <div class="flex-1">
            <div class="flex items-center gap-3 mb-4">
              <div class="w-12 h-12 bg-gradient-to-r from-blue-500 to-purple-600 rounded-lg flex items-center justify-center">
                <UIcon name="i-lucide-crown" class="w-6 h-6 text-white" />
              </div>
              <div>
                <h4 class="text-xl font-bold">{{ currentPlan.name }} Plan</h4>
                <p class="text-2xl font-bold text-primary">
                  ${{ currentPlan.price }}/{{ currentPlan.period }}
                </p>
              </div>
            </div>

            <div class="space-y-3">
              <div class="flex items-center justify-between text-sm">
                <span class="text-gray-600 dark:text-gray-400">Next billing date:</span>
                <span class="font-medium">{{ nextBillingDate }}</span>
              </div>
              <div class="flex items-center justify-between text-sm">
                <span class="text-gray-600 dark:text-gray-400">Billing cycle:</span>
                <span class="font-medium">Monthly</span>
              </div>
            </div>
          </div>

          <!-- Действия -->
          <div class="flex flex-col gap-3 flex-1 items-end">
            <UButton
                color="error"
                variant="outline"
                @click="cancelSubscription"
                class="w-fit"
            >
              <UIcon name="i-lucide-x" class="w-4 h-4 mr-2" />
              Cancel Subscription
            </UButton>
          </div>
        </div>
      </UCard>

      <!-- Функции плана -->
      <UCard class="mb-6">
        <template #header>
          <h3 class="text-lg font-semibold">Plan Features</h3>
        </template>

        <div class="flex flex-col md:flex-row gap-6">
          <!-- Включенные функции -->
          <div class="flex-1">
            <h4 class="font-medium text-green-600 dark:text-green-400 mb-3 flex items-center">
              <UIcon name="i-lucide-check-circle" class="w-4 h-4 mr-2" />
              Included Features
            </h4>
            <ul class="space-y-2">
              <li
                  v-for="feature in includedFeatures"
                  :key="feature.name"
                  class="flex items-center text-sm"
              >
                <UIcon name="i-lucide-check" class="w-4 h-4 text-green-500 mr-2" />
                {{ feature.name }}
              </li>
            </ul>
          </div>

          <!-- Не включенные функции -->
          <div class="flex-1">
            <h4 class="font-medium text-gray-500 dark:text-gray-400 mb-3 flex items-center">
              <UIcon name="i-lucide-lock" class="w-4 h-4 mr-2" />
              Premium Features
            </h4>
            <ul class="space-y-2 mb-4">
              <li
                  v-for="feature in excludedFeatures"
                  :key="feature.name"
                  class="flex items-center text-sm text-gray-500 dark:text-gray-400"
              >
                <UIcon name="i-lucide-x" class="w-4 h-4 text-gray-400 mr-2" />
                {{ feature.name }}
              </li>
            </ul>
            
            <UButton
                color="primary"
                variant="solid"
                @click="upgradePlan"
                class="w-fit"
            >
              <UIcon name="i-lucide-arrow-up" class="w-4 h-4 mr-2" />
              Upgrade to Premium
            </UButton>
          </div>
        </div>
      </UCard>

      <!-- История платежей -->
      <UCard>
        <template #header>
          <h3 class="text-lg font-semibold">Billing History</h3>
        </template>
        <div class="space-y-4">
          <div
              v-for="invoice in billingHistory"
              :key="invoice.id"
              class="flex items-center justify-between p-4 border border-gray-200 dark:border-gray-700 rounded-lg"
          >
            <div class="flex-1">
              <div class="flex items-center justify-between mb-2">
                <span class="font-medium">{{ invoice.description }}</span>
                <span class="font-medium">${{ invoice.amount }} {{ invoice.currency }}</span>
              </div>
              <div class="flex items-center justify-between text-sm text-gray-600 dark:text-gray-400">
                <span>{{ new Date(invoice.date).toLocaleDateString() }}</span>
                <div class="flex items-center gap-2">
                  <UBadge
                      :color="invoice.status === 'paid' ? 'success' : 'warning'"
                      variant="soft"
                      size="sm"
                  >
                    {{ invoice.status }}
                  </UBadge>
                  <UButton
                      color="neutral"
                      variant="ghost"
                      size="sm"
                      @click="downloadInvoice(invoice.id)"
                  >
                    <UIcon name="i-lucide-download" class="w-4 h-4" />
                  </UButton>
                </div>
              </div>
            </div>
          </div>
        </div>
      </UCard>
    </template>
  </UDashboardPanel>
</template>

<style scoped>
/* Дополнительные стили для адаптивности */
</style>