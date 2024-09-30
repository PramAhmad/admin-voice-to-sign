<template>
   <div class="w-1/6">
            <UVerticalNavigation :links="links" :ui="{
                padding: 'py-5',
            }"  />   

            <!-- button logout -->
            <div class="flex justify-center">
                <UButton
                    color="primary"
                    size="sm"
                    class="mt-5"
                    @click="Logout"
                >
                    Logout
                </UButton>
            </div>
    </div>
</template>
<script setup>
import { useJwt } from '@vueuse/integrations/useJwt'

const token = useCookie('token')
console.log(token)
const {header,payload} = useJwt(token.value)

const links = [
  [
    {
      label: payload.value.username,
      avatar: {
        src: 'https://avatars.githubusercontent.com/u/739984?v=4'
      },
    }
   ],[ {
        label: 'Dashboard',
        icon: 'i-heroicons-home',
        to: '/dashboard',
        
      
    }, {
      label: 'Assets Voice To Sign',
      icon: 'i-heroicons-chart-bar',
      to: '/assets'
    }, {
      label: 'Tambah Asset',
      icon: 'i-heroicons-command-line',
      to: '/tambah-asset'
    }
  ],
  [
// kontent edukasi
    {
      label: 'Konten Edukasi',
      icon: 'i-heroicons-light-bulb',
      to: '/content'
    },
    {
      label: 'Tambah Konten',
      icon: 'i-heroicons-question-mark-circle',
      to: '/tambah-content'
    }
  ],
  [
   
  
  ]
]


const Logout = () => {
  document.cookie = `token=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;`;
  navigateTo('/');
}

</script>
<style lang="">
    
</style>