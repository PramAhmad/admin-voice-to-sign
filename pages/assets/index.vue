<template>
    <UContainer>
      <div class="flex w-full">
        <SideBar />
        <UContainer class="ml-10 py-10 pb-16">
          <div class="w-full">
            <!-- Search Input -->
            <div class="mb-4 flex justify-center">
              <UInput
                type="text"
                placeholder="Search by name"
                v-model="searchQuery"
                @input="filterImages"
                class="w-1/2"
              />
            </div>
  
            <!-- Table -->
            <table class="w-full text-gray-300 table-auto border-collapse">
              <thead>
                <tr class="bg-slate-800 text-white">
                  <th class="py-3 px-4 text-center">No</th>
                  <th class="py-3 px-4 text-center">Image</th>
                  <th class="py-3 px-4 text-center">Text</th>

                   <th class="py-3 px-4 text-center">Create At</th>
                  <th class="py-3 px-4 text-center">Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(data, index) in paginatedImages" :key="data.ID" class="border-b border-gray-700">
                  <td class="py-3 px-4 text-center">{{ index + 1 }}</td>
                  <td class="py-3 px-4"><img :src="data.Path" alt="" width="150px" height="150px" class="rounded-md" /></td>
                  <td class="py-3 px-4 text-center min-w-[250px]">{{ data.Name }}</td>
                    <td class="py-3 px-4 text-center">{{ data.CreatedAt }}</td>
                  <td class="py-3 px-4">
                    <UButton size="sm" class="mr-2" @click="deleteImage(data.ID)">Delete</UButton>
                    <UButton size="sm" @click="editImage(data.ID)">Edit</UButton>
                  </td>
                </tr>
              </tbody>
            </table>
  
            <!-- Load More Button -->
            <div v-if="!allDataLoaded" class="mt-6 flex justify-center">
              <UButton @click="loadMore" size="md">Load More</UButton>
            </div>
          </div>
        </UContainer>
      </div>
    </UContainer>
  </template>
  <script setup>
  import axios from 'axios';
  import Swal from 'sweetalert2';
  
  const datas = ref([]);
  const paginatedImages = ref([]);
  const currentPage = ref(1);
  const perPage = 5;
  const searchQuery = ref('');
  let allDataLoaded = ref(false);
  
  const getImages = async () => {
    try {
      const res = await axios.get('https://pramudita.my.id/images');
      datas.value = res.data.result;
    //   parsing created at biat gak panjgan
        datas.value.forEach((data) => {
            data.CreatedAt = data.CreatedAt.split('T')[0];
        });
      paginateImages();
    } catch (error) {
      console.error(error);
      Swal.fire('Error', 'Failed to fetch images', 'error');
    }
  };
  
  const deleteImage = async (id) => {
    const confirmResult = await Swal.fire({
      title: 'Are you sure?',
      text: 'You won\'t be able to revert this!',
      icon: 'warning',
      showCancelButton: true,
      confirmButtonText: 'Yes, delete it!',
      cancelButtonText: 'Cancel',
      customClass: {
      confirmButton: 'bg-red-600 text-white px-4 py-2 rounded-md',
      cancelButton: 'bg-gray-300 text-black px-4 py-2 rounded-md'
    },

    });
  
    if (confirmResult.isConfirmed) {
      try {
        await axios.delete(`https://pramudita.my.id/image/${id}`);
        Swal.fire('Deleted!', 'The image has been deleted.', 'success');
        getImages();
      } catch (error) {
        Swal.fire('Error', 'Failed to delete the image', 'error');
      }
    }
  };
  
  const editImage = (id) => {
    navigateTo(`/assets/${id}`);
  };
  
  const paginateImages = () => {
    const startIndex = (currentPage.value - 1) * perPage;
    const endIndex = currentPage.value * perPage;
    paginatedImages.value = datas.value.slice(0, endIndex);
    if (paginatedImages.value.length >= datas.value.length) {
      allDataLoaded.value = true;
    }
  };
  
  const loadMore = () => {
    currentPage.value++;
    paginateImages();
  };
  
  const filterImages = () => {
    if (searchQuery.value.trim() === '') {
       
        getImages();
    } else {
      const filtered = datas.value.filter((data) =>
        data.Name.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
      paginatedImages.value = filtered.slice(0, currentPage.value * perPage);
    }
  };
  
  onMounted(() => {
    getImages();
  });
  </script>
  
  <style scoped>
  /* Add any custom styling here */
  </style>
  