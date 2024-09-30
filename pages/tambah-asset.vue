<template>
  <UContainer>
      <div class="flex w-full">
          <SideBar/>
          <section class="w-full ml-16 pt-20">
          
              <form @submit.prevent="postAsset" method="post">
                  <div class="flex flex-wrap w-full  gap-1">
                      <div class="w-1/2">
                          <label for="" class="text-white font-semibold text-lg">Nama Sign</label>
                          <UInput color="primary" v-model="nama" size="lg" class="py-5"/>
                      </div>  
                      <div class="w-1/2">
                          <label for="" class="text-white font-semibold text-lg">Path Sign</label>
                          <br>
                          <input type="file"  @change="handleFileUpload" class="text-white" />
                      </div>
                  </div>
                  <UButton color="primary" size="sm" class="mt-5" type="submit">Add</UButton>
              </form>  
          </section>
      </div>
  </UContainer>
</template>
<script setup>
import axios from 'axios';
const nama = ref('');
const path = ref(null); 

const handleFileUpload = (e) => {
    path.value = e.target.files[0];
}

const postAsset = async () => {
    console.log('nama:', nama.value);
    console.log('path:', path.value);
    const formData = new FormData();
    formData.append('Name', nama.value);
    formData.append('path', path.value);
    try {
        const res = await axios.post('https://pramudita.my.id/image', formData);
        console.log('res:', res);
        navigateTo('/assets');
    } catch (error) {
        console.error('Error posting asset:', error);
    }
} 

</script>
