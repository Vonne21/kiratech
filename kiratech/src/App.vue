<script setup>
import { ref, onMounted, computed } from 'vue'
import CardList from './components/CardList.vue'
import spinner from './components/spinner.vue'
import ModelPopup from './components/ModelPopup.vue'
import Navbar from './components/Navbar.vue'
import ProfileBanner from './components/ProfileBanner.vue'

const showSpinner = ref(false);
const currentPage = ref(1);
const itemsPerPage = 20;  

let userList = ref(null);
const genderFilter = ref('');

async function fetchUser(page = 1, button) {
  if (button == 'refresh') {
    genderFilter.value = ''
  }
  showSpinner.value = true;
  await fetch(`https://randomuser.me/api/?page=${page}&results=${itemsPerPage}&gender=${genderFilter.value}`)
    .then(response => { return response.json() })
    .then(data => {
      showSpinner.value = false;
      userList.value = data;
    }).catch(error => userList.value = error) 
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


const filterGender = (gender) => {
  genderFilter.value = gender;
  currentPage.value = 1;
  fetchUser(currentPage.value);
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
  <ProfileBanner @closeGeneralModel="closeGeneralModel" @openGeneralModel="openGeneralModel" />
  <div class="max-w-5xl mx-auto  mt-5 lg:mt-24">
    <div v-if="userList?.results && !showSpinner" class="mx-5 flex flex-col-reverse lg:flex-row gap-4 mb-4">
      <div class="flex justify-center items-center ">
        <button :disabled="currentPage <= 1" @click="changePage(currentPage - 1)"
          class=" px-4 py-1 text-xs border rounded-lg mr-2 hover:bg-cyan-500 hover:text-white disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:bg-transparent disabled:hover:text-inherit">
          Previous
        </button>
        <span class=" px-4 text-xs ">{{ currentPage }} - <span class="font-bold">{{ totalPages }}</span></span>
        <button v-if="currentPage < totalPages" @click="changePage(currentPage + 1)"
          class="px-4 py-1 text-xs border rounded-lg ml-2 mr-2 hover:bg-cyan-500 hover:text-white">
          Next
        </button>
        <button @click="fetchUser(currentPage.value, 'refresh')"
          class="flex px-4 py-1.5 text-xs border rounded-lg hover:bg-cyan-500 hover:text-white items-center"><img
            src="/refresh.png" alt="Refresh List" class="h-3"></button>
      </div>

      <div class="ml-0 lg:ml-auto flex gap-4 mb-4 justify-center lg:items-center ">
        <p>Get : </p>
        <button @click=" filterGender('female')"
          class="px-4 py-1 text-xs border rounded-lg cursor-pointer hover:bg-pink-400 hover:text-white"
          :class="genderFilter == 'female' ? 'bg-pink-400 text-white': ''">
          Female
        </button>
        <button @click="filterGender('male')"
          class="px-4 py-1 text-xs border rounded-lg cursor-pointer hover:bg-blue-400 hover:text-white"
          :class="genderFilter == 'male' ? 'bg-blue-400 text-white' : ''"
          >
          Male
        </button>
      </div>

    </div>
    <CardList v-if="userList?.results && !showSpinner" :userList="userList" @cardClicked="openPopup" />
    <spinner v-else />
  </div>
  <ModelPopup :showPopup="showPopup" :card="selectedCard" @close="closePopup" />

</template>

<style scoped></style>
