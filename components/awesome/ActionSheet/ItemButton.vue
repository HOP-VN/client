<script lang="ts" setup>
defineProps({
  text: {
    type: String,
    default: '',
  },
})
</script>

<template>
  <div
    class="flex items-center justify-center px-4 py-2 text-center hover:bg-gray-700/[0.5] active:bg-gray-700/[0.5]"
  >
    <slot>
      <span>{{ text }}</span>
    </slot>
  </div>
</template>
