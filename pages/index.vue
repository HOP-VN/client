<template>
  <div>
    <!-- <AwesomeWelcome :with-alert="true" /> -->
    <div v-for="(item, index) in data?.lstTopVideo" :key="index">
      <p>{{ item.title }}</p>
      <img :src="item.thumbnail" alt="">
    </div>
  </div>
</template>

<script setup>
import { getTopVideo, getFollowVideo, getSuggestVideo } from '~/service/index';

const { awesome } = useAppConfig()
definePageMeta({ layout: 'page' })
useHead({ titleTemplate: '', title: awesome?.name || 'Nuxt 3 Awesome Starter' })

// const { data } = await useFetch('https://graph.gplaydev.com/livestream/api/v1.0/video/top')
// console.log(data.value.Data);

const { data, error } = await useAsyncData('fetchData', async () => {
  if (process.server) {
    console.log('Running on the server');
  } else if (process.client) {
    console.log('Running on the client');
  }

  try {
    const resTopvideo = await getTopVideo();
    
    console.log("resTopvideo", resTopvideo);
    

    return {
      lstTopVideo: resTopvideo.Data
    }
  } catch (error) {
    console.log("error", error);
    return [];
  }
}, 
{
  server: false
}
)
</script>