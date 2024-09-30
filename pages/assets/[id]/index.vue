<template>
    <UContainer>
      <div class="flex w-full">
        <SideBar />
  
        <section class="w-full ml-16 pt-20">
          <img :src="data.Path" alt="" class="w-1/2" />
          <form @submit.prevent="updateAsset" method="post">
            <div class="flex flex-wrap w-full gap-1">
              <div class="w-1/2">
                <label for="" class="text-white font-semibold text-lg">Nama Sign</label>
                <UInput color="primary" v-model="nama" size="lg" class="py-5" />
              </div>
              <div class="w-1/2">
                <label for="" class="text-white font-semibold text-lg">Path Sign</label>
                <input type="file" @change="handleFileUpload" class="text-white" />
              </div>
            </div>
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
  const data = ref([]);
  const nama = ref('');
  const path = ref(null);
  
  const getDetailAsset = async () => {
    const res = await axios.get('https://pramudita.my.id/image/' + $route.params.id);
    data.value = res.data.result;
    nama.value = res.data.result.Name;
  };
  
  const updateAsset = async () => {
    console.log('nama:', nama.value);
    console.log('path:', path.value);
    
    let updateData = {
      Name: nama.value,
      Path: path.value ? path.value : null,
    };
  
    try {
      const res = await axios.put(`https://pramudita.my.id/image/${$route.params.id}`, updateData, {
        headers: path.value ? { 'Content-Type': 'multipart/form-data' } : {}
      });
      
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
  
  const handleFileUpload = (event) => {
    const file = event.target.files[0];
    path.value = file;
  };
  
  const goBack = () => {
    router.go(-1); 
  };
  
  onMounted(() => {
    getDetailAsset();
  });
  </script>
  
  <style lang="">

  </style>
  