<script setup lang="ts">
  import type { DropdownMenuItem } from '@nuxt/ui'
  interface Props {
    id?: string;
    title: string;
    menuItems?: DropdownMenuItem[] | DropdownMenuItem[][];
    notifications?: any;
  }
  const props = defineProps<Props>();
  // const emit = defineEmits(['update:modelValue', 'update:options']);
</script>

<template>
    <UDashboardNavbar :title="props.title" :ui="{ right: 'gap-3' }">
      <template #leading>
        <UDashboardSidebarCollapse />
      </template>

      <template #right>
        <UTooltip v-if="notifications" text="Notifications" :shortcuts="['N']">
          <UButton
              color="neutral"
              variant="ghost"
              square
          >
            <UChip color="error" inset>
              <UIcon name="i-lucide-bell" class="size-5 shrink-0" />
            </UChip>
          </UButton>
        </UTooltip>

        <UDropdownMenu :items="menuItems">
          <UButton icon="i-lucide-plus" size="sm" class="rounded-full" />
        </UDropdownMenu>
      </template>
    </UDashboardNavbar>
</template>

<style scoped>

</style>