<template>
    <UContainer>
      <div class="flex w-full">
        <SideBar />
  
        <section class="w-full ml-16 pt-20">
          <img :src="data.Path" alt="" class="w-1/2" />
          <form @submit.prevent="updateAsset" method="post">
            <div class="flex flex-wrap w-full gap-1">
             <div class="flex w-full gap-8">
                <div class="w-1/2">
                <label for="title" class="text-white font-semibold text-lg">Title</label>
                <UInput
                  color="primary"
                  v-model="title"
                  size="lg"
                  class="py-5"
                  placeholder="Enter the title"
                />
              </div>
              <div class="w-1/2">
                <label for="link" class="text-white font-semibold text-lg">Link</label>
                <UInput
                  color="primary"
                  v-model="link"
                  size="lg"
                  class="py-5"
                  placeholder="Enter the link"
                />
              </div>
             </div>
              <div class="w-full">
                <label for="description" class="text-white font-semibold text-lg">Description</label>
                <UInput
                  color="primary"
                  v-model="description"
                  size="lg"
                  class="py-5"
                  placeholder="Enter a description"
                />
              </div>
            </div>
            <!-- preview link using embed -->
            <iframe
              width="250"
              height="150"
              :src="link"
              title="YouTube video player"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
            <UButton color="primary" size="sm" class="mt-5" type="submit">Update</UButton>
          </form>
        </section>
      </div>
    </UContainer>
  </template>
  
  <script setup>
  import axios from 'axios';
  import Swal from 'sweetalert2';
  import { ref, onMounted } from 'vue';
  import { useRoute, useRouter } from 'vue-router';
  
  const $route = useRoute();
  const router = useRouter();
  const data = ref({});
  const title = ref('');
  const link = ref('');
  const description = ref('');
  
  const getDetailAsset = async () => {
    const res = await axios.get(`http://202.10.36.111:83/content/${$route.params.id}`);
    data.value = res.data.result;
    title.value = res.data.result.Title; 
    link.value = res.data.result.Link;     
    description.value = res.data.result.Desc; 
  };
  
  const updateAsset = async () => {
    console.log('Title:', title.value);
    console.log('Link:', link.value);
    console.log('Description:', description.value);
  
    const updateData = {
      Title: title.value,
      Link: link.value,
      Description: description.value,
    };
  
    try {
      const res = await axios.put(`http://202.10.36.111:83/content/${$route.params.id}`, updateData);
  
      Swal.fire({
        title: 'Success!',
        text: 'Data berhasil diupdate',
        icon: 'success',
        confirmButtonText: 'OK',
        customClass: {
          confirmButton: 'bg-green-500 text-white py-2 px-3 rounded-md'
        },
        buttonsStyling: false
      }).then(() => {
        router.push('/assets/' + $route.params.id);
      });
  
    } catch (error) {
      Swal.fire({
        title: 'Error!',
        text: 'Terjadi kesalahan saat mengupdate data',
        icon: 'error',
        confirmButtonText: 'OK',
        customClass: {
          confirmButton: 'bg-red-500 text-white'
        },
        buttonsStyling: false
      });
      console.error('Error updating asset:', error);
    }
  };
  
  onMounted(() => {
    getDetailAsset();
  });
  </script>
  
  <style scoped>
  /* Add any custom styles here */
  </style>
  