<script setup lang="ts">
  import type { NavigationMenuItem } from '@nuxt/ui'
  import TeamsMenu from "~/components/TeamsMenu.vue";

  const route = useRoute()
  const toast = useToast()

  const open = ref(false)

  const links = [
    [
      {
        label: 'Dasboard',
        icon: 'i-lucide-house',
        to: '/dashboard',
        onSelect: () => {
          open.value = false;
        },
      },
      {
        label: 'Transactions',
        icon: 'i-lucide-inbox',
        to: '/transactions',
        onSelect: () => {
          open.value = false;
        },
      },
      {
        label: 'Accounts',
        icon: 'i-lucide-users',
        to: '/accounts',
        onSelect: () => {
          open.value = false;
        },
      },
      {
        label: 'Investments',
        to: '/investments',
        icon: 'i-lucide-settings',
        // defaultOpen: true,
        // type: 'trigger',
        // children: [
        //   {
        //     label: 'General',
        //     to: '/settings',
        //     exact: true,
        //     onSelect: () => {
        //       open.value = false;
        //     },
        //   },
        //   {
        //     label: 'Members',
        //     to: '/settings/members',
        //     onSelect: () => {
        //       open.value = false;
        //     },
        //   },
        //   {
        //     label: 'Notifications',
        //     to: '/settings/notifications',
        //     onSelect: () => {
        //       open.value = false;
        //     },
        //   },
        //   {
        //     label: 'Security',
        //     to: '/settings/security',
        //     onSelect: () => {
        //       open.value = false;
        //     },
        //   },
        // ],
      },
      {
        label: 'Categories',
        icon: 'i-lucide-users',
        to: '/categories',
        onSelect: () => {
          open.value = false;
        },
      },
      {
        label: 'Cash flow',
        icon: 'i-lucide-users',
        to: '/cash-flow',
        onSelect: () => {
          open.value = false;
        },
      },
      {
        label: 'Goals',
        icon: 'i-lucide-users',
        to: '/goals',
        onSelect: () => {
          open.value = false;
        },
      },
      {
        label: 'Recurring',
        icon: 'i-lucide-users',
        to: '/recurring',
        onSelect: () => {
          open.value = false;
        },
      },
    ],
    [
      // {
      //   label: 'Feedback',
      //   icon: 'i-lucide-message-circle',
      //   to: 'https://github.com/nuxt-ui-pro/dashboard',
      //   target: '_blank',
      // },
      // {
      //   label: 'FAQ',
      //   icon: 'i-lucide-info',
      //   to: 'https://github.com/nuxt/ui-pro',
      //   target: '_blank',
      // },
    ],
  ] satisfies NavigationMenuItem[][];

  const groups = computed(() => [{
    id: 'links',
    label: 'Go to',
    items: links.flat()
  }, {
    id: 'code',
    label: 'Code',
    items: [{
      id: 'source',
      label: 'View page source',
      icon: 'i-simple-icons-github',
      to: `https://github.com/nuxt-ui-pro/dashboard/blob/main/app/pages${route.path === '/' ? '/index' : route.path}.vue`,
      target: '_blank'
    }]
  }])

  onMounted(async () => {
    const cookie = useCookie('cookie-consent')
    if (cookie.value === 'accepted') {
      return
    }

    toast.add({
      title: 'We use first-party cookies to enhance your experience on our website.',
      duration: 0,
      close: false,
      actions: [{
        label: 'Accept',
        color: 'neutral',
        variant: 'outline',
        onClick: () => {
          cookie.value = 'accepted'
        }
      }, {
        label: 'Opt out',
        color: 'neutral',
        variant: 'ghost'
      }]
    })
  })
</script>

<template>
  <UDashboardGroup unit="rem">
    <UDashboardSidebar
        id="default"
        v-model:open="open"
        collapsible
        resizable
        class="bg-elevated/25"
        :ui="{ footer: 'lg:border-t lg:border-default' }"
    >
      <template #header="{ collapsed }">
        <TeamsMenu :collapsed="collapsed" />
      </template>

      <template #default="{ collapsed }">
        <UDashboardSearchButton :collapsed="collapsed" class="bg-transparent ring-default" />

        <UNavigationMenu
            :collapsed="collapsed"
            :items="links[0]"
            orientation="vertical"
            tooltip
            popover
        />

        <UNavigationMenu
            :collapsed="collapsed"
            :items="links[1]"
            orientation="vertical"
            tooltip
            class="mt-auto"
        />
      </template>

      <template #footer="{ collapsed }">
        <UserMenu :collapsed="collapsed" />
      </template>
    </UDashboardSidebar>

    <UDashboardSearch :groups="groups" />

    <slot />

    <NotificationsSlideover />
  </UDashboardGroup>
</template>