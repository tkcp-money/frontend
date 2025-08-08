<script setup lang="ts">
  import AppNavbar from "~/components/AppNavbar.vue";
  
  const open = ref(false);
</script>

<template>
  <UDashboardPanel id="recurring">
    <template #header>
      <AppNavbar title="Recurring" />
    </template>
    <template #body>
      <UDrawer
          v-model="open"
          title="Drawer with sticky footer"
          description="This drawer has a sticky footer that stays visible regardless of content."
          :ui="{ 
            container: 'max-w-xl mx-auto flex flex-col h-full',
            body: 'flex-1 overflow-y-auto ttt',
            footer: 'sticky bottom-0'
          }"
      >
        <UButton 
          label="Open" 
          color="neutral" 
          variant="subtle" 
          trailing-icon="i-lucide-chevron-up"
          @click="open = true"
        />

        <template #body>
          <div class="space-y-4 p-4">
            <div class="space-y-2">
              <div v-for="i in 20" :key="i" class="p-2 bg-gray-50 rounded">
                Content item {{ i }}
              </div>
            </div>
          </div>
        </template>

        <template #footer>
          <div class="flex gap-2">
            <UButton 
              label="Submit" 
              color="primary" 
              class="flex-1 justify-center" 
            />
            <UButton
              label="Cancel"
              color="neutral"
              variant="outline"
              class="flex-1 justify-center"
              @click="open = false"
            />
          </div>
        </template>
      </UDrawer>
    </template>
  </UDashboardPanel>
</template>

<style lang="scss">
/* Additional styles for sticky footer */
.ttt {
  max-height: 50vh;
}
:deep(.u-drawer) {
  display: flex;
  flex-direction: column;
  height: 100%;
}

:deep(.u-drawer-body) {
  flex: 1;
  overflow-y: auto;
}

:deep(.u-drawer-footer) {
  flex-shrink: 0;
}
</style>