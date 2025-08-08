<script setup lang="ts">
  import AppNavbar from "~/components/AppNavbar.vue";
  // Пример данных категорий
  const navigation = [
    {
      title: 'Еда',
      budget: 20000,
      spent: 15000,
      children: [
        { title: 'Рестораны', budget: 10000, spent: 7000 },
        { title: 'Продукты', budget: 10000, spent: 8000 },
      ]
    },
    {
      title: 'Транспорт',
      budget: 5000,
      spent: 3000,
      children: [
        { title: 'Метро', budget: 2000, spent: 1000 },
        { title: 'Такси', budget: 3000, spent: 2000 },
      ]
    }
  ];
</script>

<template>
  <UDashboardPanel id="categories">
    <template #header>
      <AppNavbar title="Categories" />
    </template>
    <template #body>
      body
    </template>
  </UDashboardPanel>
</template>

<style scoped>
.categories-table {
  width: 100%;
  background: var(--color-bg);
  border-radius: 0.5rem;
  overflow: hidden;
}
.table-header {
  background: var(--color-bg-soft);
}
</style>