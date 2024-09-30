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
                placeholder="Search by title"
                v-model="searchQuery"
                @input="filterContents"
                class="w-1/2"
              />
            </div>
  
            <!-- Table -->
            <table class="w-full text-gray-300 table-auto border-collapse">
              <thead>
                <tr class="bg-slate-800 text-white">
                  <th class="py-3 px-4 text-center">No</th>
                  <th class="py-3 px-4 text-center">Title</th>
                  <th class="py-3 px-4 text-center">Link</th>
                  <th class="py-3 px-4 text-center">Description</th>
                  <th class="py-3 px-4 text-center">Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(data, index) in paginatedContents" :key="data.ID" class="border-b border-gray-700">
                  <td class="py-3 px-4 text-center">{{ index + 1 }}</td>
                  <td class="py-3 px-4 text-center min-w-[100px]">{{ data.Title }}</td>
                  <td class="py-3 px-4 text-center min-w-[250px]">
                    <!-- yt embed -->
                    <iframe
                      width="250"
                      height="150"
                      :src="data.Link"
                      title="YouTube video player"
                      frameborder="0"
                      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                      allowfullscreen
                    ></iframe>
                  </td>
                  <td class="py-3 px-4 text-left min-w-[250px]">{{ data.Desc }}</td>
                  <td class="py-3 px-4 ">
                    <div class="flex">

                        <UButton size="sm" class="mr-2" @click="deleteContent(data.ID)">Delete</UButton>
                        <UButton size="sm" @click="editContent(data.ID)">Edit</UButton>
                    </div>
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
  import { ref, onMounted } from 'vue';
  import { useRouter } from 'vue-router';
  
  const router = useRouter();
  const datas = ref([]);
  const paginatedContents = ref([]);
  const currentPage = ref(1);
  const perPage = 5;
  const searchQuery = ref('');
  let allDataLoaded = ref(false);
  
  const getContents = async () => {
    try {
      const res = await axios.get('http://202.10.36.111:83/content'); // Update this URL based on your API
      datas.value = res.data.result;
      paginateContents();
    } catch (error) {
      console.error(error);
      Swal.fire('Error', 'Failed to fetch contents', 'error');
    }
  };
  
  const deleteContent = async (id) => {
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
        await axios.delete(`http://202.10.36.111:83/content/${id}`); // Update this URL based on your API
        Swal.fire('Deleted!', 'The content has been deleted.', 'success');
        getContents();
      } catch (error) {
        Swal.fire('Error', 'Failed to delete the content', 'error');
      }
    }
  };
  
  const editContent = (id) => {
    router.push(`/content/${id}`); // Update this URL based on your routing
  };
  
  const paginateContents = () => {
    const startIndex = (currentPage.value - 1) * perPage;
    const endIndex = currentPage.value * perPage;
    paginatedContents.value = datas.value.slice(0, endIndex);
    if (paginatedContents.value.length >= datas.value.length) {
      allDataLoaded.value = true;
    }
  };
  
  const loadMore = () => {
    currentPage.value++;
    paginateContents();
  };
  
  const filterContents = () => {
    if (searchQuery.value.trim() === '') {
      getContents();
    } else {
      const filtered = datas.value.filter((data) =>
        data.Title.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
      paginatedContents.value = filtered.slice(0, currentPage.value * perPage);
    }
  };
  
  onMounted(() => {
    getContents();
  });
  </script>
  
  <style scoped>
  /* Add any custom styling here */
  </style>
  