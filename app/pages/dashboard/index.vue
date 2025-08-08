<script setup lang="ts">
  import AppNavbar from "~/components/AppNavbar.vue";

  const items = [[{
    label: 'New mail',
    icon: 'i-lucide-send',
    to: '/inbox'
  }, {
    label: 'New customer',
    icon: 'i-lucide-user-plus',
    to: '/customers'
  }]] satisfies DropdownMenuItem[][]
</script>

<template>
  <UDashboardPanel id="dashboard">
    <template #header>
      <AppNavbar title="Dashboard" :menu-items="items" />
    </template>
    <template #body>
     Dashboard body
    </template>
  </UDashboardPanel>
</template>

<style scoped>

</style>