<template>
    <UContainer>
      <div class="flex w-full">
        <SideBar />
        <section class="w-full ml-16 pt-20">
          <form @submit.prevent="postAsset" method="post">
            <div class="flex flex-wrap w-full gap-1">
            <div class="flex w-full gap-8">
                <div class="w-1/2">
                <label for="title" class="text-white font-semibold text-lg">Title</label>
                <UInput color="primary" v-model="title" size="lg" class="py-5" />
              </div>
              <div class="w-1/2">
                <label for="link" class="text-white font-semibold text-lg">Link</label>
                <UInput color="primary" v-model="link" size="lg" class="py-5" />
              </div>
            </div>
              <div class="w-full">
                <label for="description" class="text-white font-semibold text-lg">Description</label>
                <UTextarea color="primary" v-model="description" size="lg" class="py-5" /> 
            </div>
           
            </div>
            <UButton color="primary" size="sm" class="mt-5" type="submit">
              <span v-if="loading">Loading...</span>
              <span v-else>Tambah</span>
            </UButton>
          </form>
        </section>
      </div>
    </UContainer>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import axios from 'axios';
  import { useRouter } from 'vue-router'; // Assuming you're using Vue Router
  
  const router = useRouter();
  const title = ref('');
  const link = ref('');
  const description = ref('');
  const path = ref(null);
  const loading = ref(false);
  
 
  const postAsset = async () => {
   
    loading.value = true;
    const data = {
        Title: title.value,
        Link: link.value,
        Desc: description.value,
        };


    
    try {
      const res = await axios.post('https://pramudita.my.id/content', data);
      loading.value = false;
      console.log('Response:', res);
      router.push('/content'); 
    } catch (error) {
      console.error('Error posting asset:', error);
    }
  };
  </script>
  
  <style scoped>
  /* Add any custom styling here */
  </style>
  