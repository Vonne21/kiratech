<script setup>
import { ref } from 'vue'

const { userList } = defineProps(['userList']);

const formatDate = (rawDate) => {
  const date = new Date(rawDate);
  const day = date.getDate();
  const month = date.toLocaleString('default', { month: 'short' });
  const year = date.getFullYear();
  return `${day} ${month} ${year}`;
};


const emit = defineEmits(['cardClicked']);
</script>

<template>

  <div class="">
    <div class="grid grid-cols-8 gap-4 px-8 text-gray-500 text-xs">
      <div class="">Date</div>
      <div class="col-span-2">Name</div>
      <div class="">Gender</div>
      <div class="col-span-1 ">Country</div>
      <div class="">Email</div>
    </div>
    <div v-if="userList.results.length > 0">
      <div v-for="(user, index) in userList.results" :key="index" class="card mt-3" @click="emit('cardClicked', user)">
        <div class=" grid grid-cols-8 gap-4 shadow-md p-6 mb-4 rounded-md text-sm inset-ring inset-ring-gray-300/50 cursor-pointer">
          <div class=" text-gray-500">{{ formatDate(user.registered.date) }}</div>
          <div class="col-span-2 font-semibold">{{ user.name.first }} {{ user.name.last }}</div>
          <div class="text-gray-500">{{ user.gender }}</div>
          <div class="col-span-1 ">{{ user.location.country }}</div>
          <div class="text-gray-500">{{ user.email }}</div>
        </div>
      </div>
    </div>

  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
