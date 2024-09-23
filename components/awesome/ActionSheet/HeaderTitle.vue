<script lang="ts" setup>
defineProps({
  text: {
    type: String,
    default: '',
  },
})
</script>

<template>
  <div class="text-center text-xs font-bold text-gray-800 dark:text-gray-400">
    <slot>
      <span>{{ text }}</span>
    </slot>
  </div>
</template>
