<template>
    <UContainer>
        <div class="flex w-full">
       <SideBar/>
       <div class="ml-10">

           <table class="w-full ">
                <tr class="w-full">
                    <th>No</th>
                     <th>Image</th>
                     <th>Text</th>
                     <th>Action</th>
                </tr>
                <tr v-for="data,counter in datas">
                    <td class="text-gray-300 min-w-40 text-center">{{counter+1}}</td>
                     <td><img :src="data.Path" alt="" width="200px" height="200px"></td>
                     <td class="text-gray-300 min-w-40 text-center">{{data.Name}}</td>
                        <!-- action -->
                        <td>
                            <UButton size="sm" class="mr-2" @click="deleteImage(data.ID)">Delete</UButton>
                            <UButton size="sm" @click="editImage(data.ID)">Edit</UButton>
                        </td>
                </tr>
           </table>
       
       </div>
        </div>
   </UContainer>
</template>
<script setup>
import axios from 'axios';
const datas = ref([])
const getImages = async ()=>{
    const res = await axios.get('http://localhost:3000/images')
    
    datas.value = res.data.result
    console.log(datas.value)
}
const deleteImage = async (id) => {
    // cofirm
    if(confirm("yakin kan menghapus data") === true){
        const res = await axios.delete(`http://localhost:3000/images/${id}`)
        getImages()
    }   
    else{
        alert("data tidak jadi dihapus")
    }
}

const editImage = (id)=>{
    navigateTo("assets/"+id);
  
}
onMounted(() => {
    getImages()
})
</script>