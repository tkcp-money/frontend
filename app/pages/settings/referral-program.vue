<script setup lang="ts">
import type { TableColumn } from '@nuxt/ui'
// Статичные данные для приглашённых пользователей
const invitedUsers = [
  { name: 'Alice Johnson', status: 'Subscribed', reward: '$10' },
  { name: 'Bob Smith', status: 'Trial', reward: '$0' },
  { name: 'Charlie Lee', status: 'Subscribed', reward: '$10' },
  { name: 'Diana Prince', status: 'Pending', reward: '$0' },
]

const referralLink = 'https://yourapp.com/referral/abc123';

function copyReferralLink() {
  navigator.clipboard.writeText(referralLink)
}

const columns: TableColumn<{ name: string; status: string; reward: string; }>[] = [
  { accessorKey: 'name', header: 'Name' },
  { accessorKey: 'status', header: 'Status' },
  { accessorKey: 'reward', header: 'Reward' }
]
</script>

<template>
  <div class="referral-page space-y-8">
    <!-- Заголовок и описание -->
    <UCard class="p-6">
      <div class="flex flex-col gap-2">
        <h1 class="text-2xl font-bold">Invite friends and earn rewards</h1>
        <p class="text-gray-500">Invite 3 friends and get a free subscription</p>
      </div>
    </UCard>

    <!-- How it works -->
    <UCard class="p-6">
      <h2 class="text-lg font-semibold mb-4">How it works</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div class="flex flex-col items-center text-center">
          <UIcon name="i-heroicons-link" class="text-primary mb-2 text-3xl" />
          <span class="font-medium">Share your referral link</span>
        </div>
        <div class="flex flex-col items-center text-center">
          <UIcon name="i-heroicons-user-plus" class="text-primary mb-2 text-3xl" />
          <span class="font-medium">Friend subscribes</span>
        </div>
        <div class="flex flex-col items-center text-center">
          <UIcon name="i-heroicons-gift" class="text-primary mb-2 text-3xl" />
          <span class="font-medium">You get a reward</span>
        </div>
      </div>
    </UCard>

    <!-- Список приглашённых пользователей -->
    <UCard class="p-6">
      <h2 class="text-lg font-semibold mb-4">Invited Friends</h2>
      <UTable :rows="invitedUsers" :columns="columns" class="mb-2" />
    </UCard>

    <!-- Реферальная ссылка -->
    <UCard class="p-6 flex flex-col gap-4">
      <h2 class="text-lg font-semibold">Your referral link</h2>
      <div class="flex items-center gap-2">
        <UInput :model-value="referralLink" readonly class="flex-1" />
        <UButton icon="i-heroicons-document-duplicate" @click="copyReferralLink" color="primary" variant="solid">Copy</UButton>
      </div>
    </UCard>

    <!-- Оффер блок -->
    <UCard class="p-6 offer-block flex flex-col md:flex-row items-center justify-between gap-4 bg-primary-50 border-primary">
      <div class="flex items-center gap-3">
        <UIcon name="i-heroicons-sparkles" class="text-primary text-2xl" />
        <span class="font-semibold text-primary">Just 3 invites and your plan is covered!</span>
      </div>
      <UButton color="primary" size="lg">Invite Now</UButton>
    </UCard>
  </div>
</template>

<style scoped>
.referral-page {
  max-width: 700px;
  margin: 0 auto;
}
.offer-block {
  border-width: 2px;
}
</style> 