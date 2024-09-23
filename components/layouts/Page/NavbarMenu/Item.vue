<script lang="ts" setup>
const nuxtApp = useNuxtApp()
const { parseMenuRoute, parseMenuTitle } = useNavbarParser()

const props = defineProps({
  menu: {
    type: Object as () =>
      | AwesomeLayoutPageNavbarMenu
      | AwesomeLayoutPageNavbarMenuDropdownItem,
    required: true,
  },
  isDropdown: {
    type: Boolean,
    default: false,
  },
})
</script>

<template>
  <template v-if="menu?.type === 'link' && isDropdown">
    <NuxtLink :to="parseMenuRoute(menu?.to)" #="{ isActive }">
      <div
        :class="[
          'w-full rounded-lg px-4 py-2 transition-all duration-300 hover:bg-gray-100 dark:hover:bg-gray-800',
          isActive
            ? 'font-bold text-gray-900 dark:text-gray-100'
            : 'text-gray-700 dark:text-gray-300',
        ]"
      >
        {{ parseMenuTitle(menu?.title) }}
      </div>
    </NuxtLink>
  </template>
  <template v-else-if="menu?.type === 'link'">
    <NuxtLink :to="parseMenuRoute(menu?.to)" #="{ isActive }">
      <span
        :class="{
          'font-bold text-gray-900 dark:text-gray-100': isActive,
          'text-gray-700 dark:text-gray-300': !isActive,
        }"
        >{{ parseMenuTitle(menu?.title) }}</span
      >
    </NuxtLink>
  </template>
  <template v-else-if="menu?.type === 'button'">
    <AwesomeButton
      :text="parseMenuTitle(menu?.title)"
      size="xs"
      :to="parseMenuRoute(menu.to)"
    />
  </template>
</template>
