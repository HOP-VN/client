<script lang="ts" setup>
const { awesome } = useAppConfig()
</script>

<template>
  <footer class="flex border-t border-gray-900/10 dark:border-gray-50/[0.2]">
    <div
      class="mx-auto flex max-w-screen-2xl flex-1 flex-col items-center justify-between space-y-2 px-4 py-2 text-center text-xs text-gray-400 md:flex-row md:space-y-0 md:text-left"
    >
      <div>
        Copyright ©
        {{ awesome?.layout?.footer?.year || new Date().getFullYear() }}
        {{ awesome?.author?.name || '' }}. All rights reserved.
      </div>
      <div>{{ awesome.name }}</div>
    </div>
  </footer>
</template>
