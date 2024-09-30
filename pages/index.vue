<template class="bg-gray-900">
  <div class="w-full bg-slate-950 h-screen">
    
    <div class="flex w-full  justify-center items-center h-screen">
      <UForm class="md:w-1/3 w-full bg-slate-900 rounded-md p-5" @submit="Login">
        <h3 class="text-white text-2xl text-center font-semibold pb-10">Ananta VoiceToSign</h3>
        
        <UFormGroup label="username" name="email">
          <UInput size="lg" class="py-3" v-model="email" />
        </UFormGroup>
    
        <UFormGroup label="Password" name="password">
          <UInput type="password" size="lg" class="py-3" v-model="password" />
        </UFormGroup>
    
        <UButton type="submit" class="mt-3" size="md">Login</UButton>
      </UForm>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import Swal from "sweetalert2";
const email = ref("");
const password = ref("");

const Login = async () => {
  try {
    const response = await fetch('https://pramudita.my.id/login', {
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
    
    if (response.status === 200 || response.status === 302) {
      const token = res.token;
      document.cookie = `token=${token}`;
      
      // Success Swal message
      Swal.fire({
        title: 'Login Successful!',
        text: 'Redirecting to dashboard...',
        icon: 'success',
        timer: 2000,
        showConfirmButton: false,
      }).then(() => {
        navigateTo('/dashboard');
      });

    } else {
      // Error Swal message
      Swal.fire({
        title: 'Login Failed!',
        text: res.error || 'Please check your credentials',
        icon: 'error',
        confirmButtonText: 'Try Again',
      });
    }
    
  } catch (error) {
    // Catching and showing general error with Swal
    Swal.fire({
      title: 'Error',
      text: 'An error occurred during the login process.',
      icon: 'error',
      confirmButtonText: 'OK',
    });
    console.error(error);
  }
};

const getImage = async () => {
  const res = await axios.get('https://60jwg094-3000.asse.devtunnels.ms/images');
  console.log(res);
};

onMounted(() => {
  getImage();
});
</script>
