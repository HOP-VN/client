<script lang="ts" setup>
// micro compiler
const props = defineProps({
  text: {
    type: String,
    default: '',
  },
  to: {
    type: [String, Object],
    default: undefined,
  },
  href: {
    type: String,
    default: '',
  },
})
// state
const href = toRef(props, 'href')
const to = toRef(props, 'to')
</script>

<template>
  <NuxtLink
    v-if="to"
    tag="a"
    :to="to"
    :class="`transition-colors duration-300 hover:text-gray-900 hover:underline dark:hover:text-white`"
  >
    <slot>{{ text }}</slot>
  </NuxtLink>
  <a
    v-else-if="!href"
    :class="`transition-colors duration-300 hover:text-gray-900 hover:underline dark:hover:text-white`"
    href="#"
  >
    <slot>{{ text }}</slot>
  </a>
  <a
    v-else
    :class="`transition-colors duration-300 hover:text-gray-900 hover:underline dark:hover:text-white`"
    :href="href"
  >
    <slot>{{ text }}</slot>
  </a>
</template>
