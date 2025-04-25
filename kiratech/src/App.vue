<script setup>
import { ref, onMounted, computed } from 'vue'
import CardList from './components/CardList.vue'
import spinner from './components/spinner.vue'
import ModelPopup from './components/ModelPopup.vue'
import GeneralModel from './components/GeneralModel.vue'

const showSpinner = ref(false);
const currentPage = ref(1);
const itemsPerPage = 20;  

let userList = ref(null);
const genderFilter = ref('');

async function fetchUser(page = 1) {
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

const showGeneralModel = ref(false)
const typeOfModel = ref('')

const openGeneralModel = (type) => {
  typeOfModel.value = type;
  showGeneralModel.value = true;
};
const closeGeneralModel = () => {
  showGeneralModel.value = false;
};



</script>

<template>
  <div class="bg-cyan-500 w-full min-h-[154px]">
    <div class="max-w-5xl mx-auto flex">
      <img src="/Avatar.png" alt="" class="w-36 h-36 relative top-14">
      <div class="flex flex-col text-white self-end mb-2 ml-5">
        <p class="font-bold text-2xl">John Doe</p>
        <p>Last Online 2 days</p>
      </div>
      <div class="flex flex-row self-end ml-4 mb-2">
        <button
          class="bg-white px-4 py-2 text-xs font-semibold text-cyan-500 rounded-lg h-fit flex mx-2 items-center cursor-pointer"
          @click="openGeneralModel('send')">
          <img src="/send.png" alt="" class="h-3 mr-2">
          <p>Send Message</p>
        </button>
        <button
          class="outline outline-white px-4 py-2 text-xs font-semibold text-white rounded-lg h-fit flex items-center cursor-pointer"
          @click="openGeneralModel('add')">
          <img src="/add.png" alt="" class="h-3 mr-2">
          <p>Add Friend</p>
        </button>
      </div>
    </div>
  </div>

  <div class="max-w-5xl mx-auto mt-24">

    <div v-if="userList?.results && !showSpinner" class="flex gap-4 mb-4">
      <div class="flex justify-center items-center ">
        <button v-if="currentPage > 1" @click="changePage(currentPage - 1)"
          class="px-4 py-1 text-xs border rounded-lg mr-2 hover:bg-cyan-500 hover:text-white">
          Previous
        </button>
        <span class=" px-4 text-xs ">{{ currentPage }} / {{ totalPages }}</span>
        <button v-if="currentPage < totalPages" @click="changePage(currentPage + 1)"
          class="px-4 py-1 text-xs border rounded-lg ml-2 hover:bg-cyan-500 hover:text-white">
          Next
        </button>
      </div>

      <div class="ml-auto flex gap-4 mb-4 items-center">
        <button @click="fetchUser"
          class="flex px-4 py-1 text-xs border rounded-lg mr-2 hover:bg-cyan-500 hover:text-white items-center"><img
            src="/refresh.png" alt="" class="h-3 mr-2">Refresh List</button>
        <button @click=" filterGender('female')"
          class="px-4 py-1 text-xs border rounded-lg cursor-pointer hover:bg-pink-400 hover:text-white">
          Female
        </button>
        <button @click="filterGender('male')"
          class="px-4 py-1 text-xs border rounded-lg cursor-pointer hover:bg-blue-400 hover:text-white">
          Male
        </button>
      </div>

    </div>
    <CardList v-if="userList?.results && !showSpinner" :userList="userList" @cardClicked="openPopup" />
    <spinner v-else />
  </div>



  <ModelPopup :showPopup="showPopup" :card="selectedCard" @close="closePopup" />
  <GeneralModel :showGeneralModel="showGeneralModel" :typeOfModel="typeOfModel"
    @closeGeneralModel="closeGeneralModel" />

</template>

<style scoped></style>
