
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
    const response = await fetch('http://202.10.36.111:83/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        Username: email.value,
        Password: password.value,
      }),
    });

    const res = await response.json();
    console.log('data'+res)
    if (response.status === 200 || response.status === 302) {
      const token = res.token;
      document.cookie = `token=${token}`;

      navigateTo('/dashboard');
      
    } else {
      alert(res);
    }
  } catch (error) {
    alert("An error occurred during the fetch operation.");
    console.error(error);
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
