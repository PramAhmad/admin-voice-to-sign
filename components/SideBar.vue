<template>
   <div class="w-1/6">
            <UVerticalNavigation :links="links" :ui="{
                padding: 'py-5',
            }"  />   
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
   
    {
      label: 'Logout',
      icon: 'i-heroicons-question-mark-circle',
      to: '/getting-started/examples',

    }
  ]
]

</script>
<style lang="">
    
</style>