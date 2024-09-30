<template>
    <UContainer>
        <div class="flex w-full">
            <SideBar/>
            <section class="w-full ml-16 pt-20">
                <form @submit.prevent="postAsset" method="post">
                    <div class="flex flex-wrap w-full gap-1">
                        <div class="w-1/2">
                            <label for="" class="text-white font-semibold text-lg">Nama Sign</label>
                            <UInput color="primary" v-model="nama" size="lg" class="py-5" :disabled="loading"/>
                        </div>  
                        <div class="w-1/2">
                            <label for="" class="text-white font-semibold text-lg">Path Sign</label>
                            <br>
                            <input type="file" @change="handleFileUpload" class="text-white" :disabled="loading"/>
                        </div>
                    </div>
                    <UButton color="primary" size="sm" class="mt-5" type="submit" :disabled="loading">
                        <span v-if="loading">Loading...</span>
                        <span v-else>Tambah</span>
                    </UButton>
                </form>  
            </section>
        </div>
    </UContainer>
  </template>
  
  <script setup>
  import axios from 'axios';
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  
  const nama = ref('');
  const path = ref(null);
  const loading = ref(false);
  const router = useRouter();
  
  const handleFileUpload = (e) => {
      path.value = e.target.files[0];
  }
  
  const postAsset = async () => {
      loading.value = true; 
      const formData = new FormData();
      formData.append('Name', nama.value);
      formData.append('path', path.value);
  
      try {
          const res = await axios.post('https://pramudita.my.id/image', formData);
          console.log('res:', res);
          router.push('/assets');
      } catch (error) {
          console.error('Error posting asset:', error);
      } finally {
          loading.value = false; 
      }
  }
  </script>
  