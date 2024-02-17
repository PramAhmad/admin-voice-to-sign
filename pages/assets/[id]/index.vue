<template>
    <UContainer>
        <div class="flex w-full">
            <SideBar/>
            <section class="w-full ml-16 pt-20">
                <img :src="data.Path" alt="" class="w-1/2">
                <form @submit.prevent="updateAsset" method="post">
                    <div class="flex flex-wrap w-full  gap-1">
                        <div class="w-1/2">
                            <label for="" class="text-white font-semibold text-lg">Nama Sign</label>
                            <UInput color="primary" v-model="nama" size="lg" class="py-5"/>
                        </div>  
                        <div class="w-1/2">
                            <label for="" class="text-white font-semibold text-lg">Path Sign</label>
                            <input type="file"  @change="handleFileUpload" class="text-white" />
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

const $route = useRoute();
const data = ref([]);
const nama = ref('');
const path = ref(null); // Initialize path as null

const getDetailAsset = async () => {
    const res = await axios.get('http://localhost:3000/image/' + $route.params.id);
    data.value = res.data.result;
    nama.value = res.data.result.Name;
  
}

const updateAsset = async () => {
    console.log('nama:', nama.value);
    console.log('path:', path.value);
    // jika file udah ada gak usah di update filenya
    if (path.value === null) {
        console.log('path is null');    
        try {
            const res = await axios.put(`http://localhost:3000/image/${$route.params.id}`, {
                Name: nama.value,
                Path: null,
            });
          navigateTo('/assets'+$route.params.id)
        } catch (error) {
            console.error('Error updating asset:', error);
        }
        return;
    }
    else{

  
        try {
            const res = await axios.put(`http://localhost:3000/image/${$route.params.id}`, {
                Name: nama.value,
                Path: path.value
            
            }, {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
            });
            navigateTo('/assets'+$route.params.id)
        } catch (error) {
            console.error('Error updating asset:', error);
        }
    }
    }

const handleFileUpload = () => {
    const file = event.target.files[0];
    path.value = file;
}

onMounted(() => {
    getDetailAsset();
});
</script>

<style lang="">
    
</style>
