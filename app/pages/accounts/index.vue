<script setup lang="ts">
  import AppNavbar from "~/components/AppNavbar.vue";
  import type { DropdownMenuItem } from '@nuxt/ui';
  import { AddAccountModal } from './_components';

  const menuItems = ref<DropdownMenuItem[]>([
    {
      label: 'Cash',
      // icon: 'i-lucide-dollar-sign',
      children: [
        [
          {
            label: 'Track manually',
            // icon: 'i-lucide-pencil'
            onClick: () => openAddAccountModal('cash'),
          }
        ]
      ]
    },
    {
      label: 'Credit Card, Checking, Savings',
      // icon: 'i-lucide-credit-card',
      children: [
        [
          {
            label: 'Link an institution',
            // icon: 'i-lucide-link'
            onClick: () => openAddAccountModal('cash'),
          },
          {
            label: 'Track manually',
            // icon: 'i-lucide-pencil'
            onClick: () => openAddAccountModal('cash'),
          }
        ]
      ]
    },
    // {
    //   label: 'Real Estate',
    //   // icon: 'i-lucide-home',
    //   children: [
    //     [
    //       {
    //         label: 'Connect a property',
    //         // icon: 'i-lucide-building'
    //         onClick: () => openAddAccountModal('cash'),
    //       },
    //       {
    //         label: 'Track manually',
    //         // icon: 'i-lucide-pencil'
    //         onClick: () => openAddAccountModal('cash'),
    //       }
    //     ]
    //   ]
    // },
    {
      label: 'Loans',
      // icon: 'i-lucide-banknote',
      children: [
        [
          {
            label: 'Link an institution',
            // icon: 'i-lucide-link'
            onClick: () => openAddAccountModal('cash'),
          },
          {
            label: 'Track manually',
            // icon: 'i-lucide-pencil'
            onClick: () => openAddAccountModal('cash'),
          }
        ]
      ]
    }
  ])
  const isAddAccountModalOpen = ref<boolean>(false);

  function openAddAccountModal(accountType: string) {
    console.log(accountType);
    isAddAccountModalOpen.value = true;
  }
</script>

<template>
    <UDashboardPanel id="accounts">
      <template #header>
        <AppNavbar title="Accounts" :menu-items="menuItems"  />
      </template>
      <template #body>
        {{ isAddAccountModalOpen }}
        <AddAccountModal v-model="isAddAccountModalOpen" />
      </template>
    </UDashboardPanel>

</template>

<style scoped>

</style>