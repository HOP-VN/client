<script lang="ts" setup>
const { awesome } = useAppConfig()
const { parseMenuRoute, parseMenuTitle } = useNavbarParser()
const $screen = useAwesomeScreen()
const nuxtApp = useNuxtApp()

const menus = computed(
  () =>
    (awesome?.layout?.page?.navbar?.menus ||
      []) as AwesomeLayoutPageNavbarMenu[]
)

// drawer
const showDrawer = ref(false)
</script>

<template>
  <header
    class="fixed top-0 z-40 flex w-full flex-none border-b border-gray-950/10 bg-white/[0.5] backdrop-blur-md backdrop-filter transition-colors duration-300 dark:border-gray-50/[0.2] dark:bg-gray-950/[0.5] lg:z-50"
  >
    <!-- content -->
    <div
      class="mx-auto flex max-w-screen-2xl flex-1 items-center justify-between px-4"
    >
      <!-- title -->
      <div>
        <slot name="title">
          <NuxtLink to="/" class="text-lg font-bold text-primary-500">
            <Icon
              name="simple-icons:nuxtdotjs"
              class="mr-2 inline-block font-mono text-xl font-black"
            />
            <span class="capitalize">{{ awesome.name }}</span>
          </NuxtLink>
        </slot>
      </div>
      <!-- menus -->
      <div
        v-if="$screen.higherThan('md', $screen.current.value)"
        class="flex items-center space-x-4"
        :class="{ 'divide-x divide-gray-500': menus.length > 0 }"
      >
        <div class="flex items-center space-x-4 text-sm">
          <!-- dynamic menus -->
          <template v-for="(item, i) in menus" :key="i">
            <LayoutPageNavbarMenuWrapper :menu="item" />
          </template>
        </div>
        <!-- others -->
        <div class="flex space-x-3 pl-4 text-xl">
          <!-- todo: feat/localization -->
          <!-- <AwesomeLink class="text-gray-400 hover:text-gray-100">
            <Icon name="la:language" />
          </AwesomeLink> -->
          <LayoutPageNavbarDropdownThemeSwitcher />
          <AwesomeLink
            v-if="awesome?.project?.links?.github"
            class="text-gray-600 dark:text-gray-400"
            :href="awesome?.project?.links?.github"
          >
            <Icon name="mdi:github-face" />
          </AwesomeLink>
        </div>
      </div>
      <!-- drawer:btn -->
      <div
        v-else
        class="flex items-center space-x-4"
        :class="{ 'divide-x divide-gray-500': menus.length > 0 }"
      >
        <div class="flex space-x-3 pl-4 text-xl">
          <AwesomeLink
            v-if="awesome?.project?.links?.github"
            class="text-gray-400 hover:text-gray-100"
            @click.prevent="() => (showDrawer = !showDrawer)"
          >
            <Icon name="heroicons:bars-3-bottom-right-20-solid" />
          </AwesomeLink>
        </div>
      </div>
    </div>
    <!-- misc -->
    <!-- drawer -->
    <AwesomeActionSheet
      v-if="!$screen.higherThan('md', $screen.current.value) && showDrawer"
      @close="() => (showDrawer = false)"
    >
      <AwesomeActionSheetGroup>
        <AwesomeActionSheetHeader>
          <AwesomeActionSheetHeaderTitle text="Menu" />
        </AwesomeActionSheetHeader>
        <!-- dynamic menus -->
        <AwesomeActionSheetItem>
          <div
            class="flex flex-col items-center divide-y divide-gray-400 text-center text-sm dark:divide-gray-700"
          >
            <template v-for="(item, i) in menus">
              <template v-if="item?.type === 'link'">
                <NuxtLink
                  :key="i"
                  :to="parseMenuRoute(item.to)"
                  #="{ isActive }"
                  class="w-full py-2"
                >
                  <span
                    :class="{
                      'font-bold text-gray-900 dark:text-gray-100': isActive,
                      'text-gray-700 dark:text-gray-300': !isActive,
                    }"
                    >{{ parseMenuTitle(item?.title) }}</span
                  >
                </NuxtLink>
              </template>
              <template v-if="item?.type === 'button'">
                <AwesomeButton
                  :key="i"
                  :text="parseMenuTitle(item?.title)"
                  size="sm"
                  :to="parseMenuRoute(item.to)"
                  class="w-full"
                />
              </template>
              <template v-if="item?.type === 'dropdown'">
                <div :key="i" class="w-full">
                  <HeadlessDisclosure v-slot="{ open }">
                    <HeadlessDisclosureButton
                      :key="i"
                      :class="[
                        'flex w-full items-center justify-center py-2 text-gray-700 transition-all duration-300 dark:text-gray-300',
                        open ? 'font-bold' : '',
                      ]"
                    >
                      <span>{{ parseMenuTitle(item?.title) }}</span>
                      <Icon
                        name="carbon:chevron-right"
                        class="ml-1"
                        :class="[
                          open
                            ? 'rotate-90 transform transition-all duration-300'
                            : 'rotate-0',
                        ]"
                      />
                    </HeadlessDisclosureButton>
                    <Transition
                      enter-active-class="transition duration-100 ease-out"
                      enter-from-class="transform scale-95 opacity-0"
                      enter-to-class="transform scale-100 opacity-100"
                      leave-active-class="transition duration-75 ease-out"
                      leave-from-class="transform scale-100 opacity-100"
                      leave-to-class="transform scale-95 opacity-0"
                    >
                      <HeadlessDisclosurePanel class="pb-2 text-gray-500">
                        <template
                          v-for="(child, j) in item?.children || []"
                          :key="j"
                        >
                          <NuxtLink
                            :to="parseMenuRoute(child.to)"
                            #="{ isActive }"
                            class="w-full py-2"
                          >
                            <span
                              :class="[
                                isActive
                                  ? 'font-bold text-gray-900 dark:text-gray-100'
                                  : 'text-gray-700 dark:text-gray-300',
                              ]"
                              >{{ parseMenuTitle(child?.title) }}</span
                            >
                          </NuxtLink>
                        </template>
                      </HeadlessDisclosurePanel>
                    </Transition>
                  </HeadlessDisclosure>
                </div>
              </template>
            </template>
          </div>
        </AwesomeActionSheetItem>
        <AwesomeActionSheetItem class="flex flex-col">
          <div class="pb-2">
            <div class="mb-2 mt-2 text-sm font-bold capitalize">
              Change Theme
            </div>
            <LayoutPageNavbarDropdownThemeSwitcher type="select-box" />
          </div>
        </AwesomeActionSheetItem>
      </AwesomeActionSheetGroup>
      <AwesomeActionSheetGroup>
        <AwesomeActionSheetItemButton
          class="flex items-center justify-center space-x-2 text-base"
        >
          <Icon name="mdi:github-face" class="text-lg font-bold" />
          <span class="text-sm">Github</span>
        </AwesomeActionSheetItemButton>
      </AwesomeActionSheetGroup>
    </AwesomeActionSheet>
  </header>
</template>
