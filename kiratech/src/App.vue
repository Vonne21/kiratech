<script setup>
import { ref, onMounted, computed } from 'vue'
import CardList from './components/CardList.vue'
import spinner from './components/spinner.vue'
import ProfileModelPopup from './components/ProfileModelPopup.vue'
import Navbar from './components/Navbar.vue'
import ProfileBanner from './components/ProfileBanner.vue'

const showSpinner = ref(false);
const currentPage = ref(1);
const itemsPerPage = 20;  

let userList = ref(null);
const genderFilter = ref('');

async function fetchUser(page = 1, button) {
  
  if (button == 'refresh') {
    // for fresh refresh
    genderFilter.value = '';
  } else if (button == 'female') {
    genderFilter.value = 'female';
  } else if (button == 'male') {
    genderFilter.value = 'male';
  }
  
  showSpinner.value = true;
  
  const params = new URLSearchParams({
    page: page,
    results: itemsPerPage,
  });

  if (genderFilter.value) {
    params.append('gender', genderFilter.value);
  }

  try {
    const response = await fetch(`https://randomuser.me/api/?${params.toString()}`);
    const data = await response.json();
    userList.value = data;
  } catch (error) {
    userList.value = error;
  } finally {
    showSpinner.value = false;
  }
}

onMounted(() => {
  fetchUser(currentPage.value);
});


const showPopup = ref(false);
const selectedCard = ref(null);

const openPopup = (card) => {
  selectedCard.value = card;
  showPopup.value = true;
};

const closePopup = () => {
  showPopup.value = false;
};

const totalPages = computed(() => {
  return 3; 
});

const changePage = (page) => {
  currentPage.value = page;
  fetchUser(currentPage.value);
};

</script>

<template>
  <Navbar />
  <ProfileBanner />
  <div class="max-w-5xl mx-auto  mt-5 lg:mt-24">
    <div v-if="userList?.results && !showSpinner" class="mx-7 flex flex-col-reverse lg:flex-row gap-4 mb-4">
      <div class="flex justify-center items-center border-t lg:border-0 pt-4 lg:pt-0 border-cyan-500">
        <button :disabled="currentPage <= 1" @click="changePage(currentPage - 1)"
          class=" px-4 py-1 text-xs border rounded-lg mr-2 hover:bg-cyan-500 hover:text-white disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:bg-transparent disabled:hover:text-inherit min-w-[79px]">
          Previous
        </button>
        <span class=" px-4 text-xs ">{{ currentPage }} - <span class="font-bold">{{ totalPages }}</span></span>
        <button :disabled="!(currentPage < totalPages)" @click="changePage(currentPage + 1)"
          class="px-4 py-1 text-xs border rounded-lg ml-2 mr-2 hover:bg-cyan-500 hover:text-white disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:bg-transparent disabled:hover:text-inherit min-w-[79px]">
          Next
        </button>
        <button @click="fetchUser(currentPage = 1, 'refresh')"
          class="flex px-4 py-1.5 text-xs border rounded-lg hover:bg-cyan-500 hover:text-white items-center"><img
            src="/refresh.png" alt="Refresh List" class="h-3"></button>
      </div>

      <div class="ml-0 lg:ml-auto flex gap-4 lg:mb-4 justify-center lg:items-center ">
        <p>Get : </p>
        <button @click=" fetchUser(currentPage = 1, 'female')"
          class="px-4 py-1 text-xs border rounded-lg cursor-pointer hover:bg-pink-400 hover:text-white min-w-[72px]"
          :class="genderFilter == 'female' ? 'bg-pink-400 text-white': ''">
          Female
        </button>
        <button @click="fetchUser(currentPage = 1, 'male')"
          class="px-4 py-1 text-xs border rounded-lg cursor-pointer hover:bg-blue-400 hover:text-white min-w-[72px]"
          :class="genderFilter == 'male' ? 'bg-blue-400 text-white' : ''">
          Male
        </button>
      </div>

    </div>
    <CardList v-if="userList?.results && !showSpinner" :userList="userList" @cardClicked="openPopup" />
    <spinner v-else />
  </div>
  <ProfileModelPopup :showPopup="showPopup" :card="selectedCard" @close="closePopup" />

</template>

<style scoped></style>
