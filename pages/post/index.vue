<script lang="ts" setup>
// compiler micro
definePageMeta({ layout: 'page' })
useHead({ title: 'Posts' })
// query
// const query: QueryBuilderParams = { path: '/post' }
</script>

<template>
  <LayoutPageWrapper>
    <LayoutPageHeader>
      <LayoutPageTitle text="Posts" class="capitalize" />
    </LayoutPageHeader>
    <LayoutPageSection>
      <ContentList path="/post">
        <template #default="{ list }">
          <div
            v-for="article in list"
            :key="article._path"
            class="mb-4 flex space-x-6 rounded border border-gray-900/10 p-6 hover:no-underline dark:border-gray-50/[0.2]"
          >
            <div class="mt-1 text-right text-gray-600 dark:text-gray-400">
              <div>{{ article.date }}</div>
              <AwesomeLink
                class="flex items-center justify-end space-x-1 text-sm"
                :href="`https://www.github.com/${article.author}`"
              >
                <Icon name="mdi:github-face" class="text-xs" />
                <span>{{ article.author }}</span>
              </AwesomeLink>
            </div>
            <div class="flex flex-col">
              <div
                class="text-xl font-semibold text-gray-800 dark:text-gray-50"
              >
                {{ article.title }}
              </div>
              <div class="mb-1 text-gray-700 dark:text-gray-300">
                {{ article.description }}
              </div>
              <div class="flex">
                <AwesomeLink
                  class="flex items-center space-x-1 text-sm text-primary-500"
                  :to="article._path"
                >
                  <span>learn more</span>
                  <Icon name="ic:baseline-arrow-right-alt" class="text-sm" />
                </AwesomeLink>
              </div>
            </div>
          </div>
        </template>
        <template #not-found>
          <p>No articles found.</p>
        </template>
      </ContentList>
    </LayoutPageSection>
  </LayoutPageWrapper>
</template>
