<template>
    <UContainer>
        <div class="flex w-full">


            <SideBar/>
           <UContainer>
        
            <div class="p-10 items-start w-full">

                <UCard class="min-w-[900px]" >
                    <div class="flex items-center w-full justify-center flex-col">

                        <UIcon name="i-heroicons-photo" class="text-7xl  text-gray-500" />
                        <h2 class="text-green-500 font-bold text-3xl ">{{data}}</h2>
                        <h3 class="text-gray-500 font-semibold text-xl">Total Assets Image</h3>
                    </div>
                </UCard>
                <UCard class="min-w-[900px] mt-10">
                    <div class="flex items-center w-full justify-center flex-col">

                    <UIcon name="i-heroicons-video-camera" class="text-7xl  text-gray-500" />
                    <h2 class="text-green-500 font-bold text-3xl ">231</h2>
                    <h3 class="text-gray-500 font-semibold text-xl">Total Assets Video</h3>
                    </div>
                </UCard> 
            </div>
           </UContainer>
        </div>
    </UContainer>
</template>
<script setup>
import axios from "axios"
const data = ref()
const CountData = async ()=>{
    const res = await axios.get("http://localhost:3000/images")
    data.value = res.data.count
    
}
onMounted(()=>{
    CountData()
})
</script>
<style lang="">
    
</style>