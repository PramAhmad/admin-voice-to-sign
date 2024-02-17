
<template class="bg-gray-900">
  <div class="w-full bg-slate-950 h-screen">
    
    <div class="flex w-full  justify-center items-center h-screen ">
  
      <UForm class="md:w-1/3 w-full bg-slate-900 rounded-md p-5 " @submit="Login">
        <h3 class="text-white text-2xl text-center font-semibold pb-10">Ananta VoiceToSign</h3>
        <UFormGroup label="Email" name="email" class="" >
          <UInput size="lg" class="py-3" v-model="email"/>
        </UFormGroup>
    
        <UFormGroup label="Password" name="password"  >
          <UInput  type="password" size="lg" class="py-3" v-model="password"/>
        </UFormGroup>
    
        <UButton type="submit" class="mt-3" size="md">
          Login
        </UButton>
      </UForm>
    </div>
  </div>
</template>
<script setup>
import axios from "axios"
const email  = ref('')
const password = ref('')
const Login = async () => {
  try {
    const res = await axios.post('http://localhost:3000/login', {
      Username: email.value,
      Password: password.value
    });

    if (res.status === 200 || res.status === 302) {
      const token = res.data.token;
      document.cookie = `token=${token}`;
      
      navigateTo('/dashboard');
      
    } else{
      alert(res.data)
    }
  } catch (error) {
      if(error.response){
        alert(error.response.data.error)
        console.log(error.response.data.error)
      }
      else if (error.request) {
        alert("gada dari server")
      }
  }
};


const getImage = async () => {
  const res = await axios.get('https://60jwg094-3000.asse.devtunnels.ms/images')
  console.log(res)
}

onMounted(()=>{
  getImage()
})

</script>
