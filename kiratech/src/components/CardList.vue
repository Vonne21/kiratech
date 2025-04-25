<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  userList: {
    type: Object,
    required: true
  }
});
const formatDate = (rawDate) => {
  const date = new Date(rawDate);
  const day = date.getDate();
  const month = date.toLocaleString('default', { month: 'short' });
  const year = date.getFullYear();
  return `${day} ${month} ${year}`;
};

const sortedUsers = ref([...props.userList.results]);
const sortAsc = ref(true);

const sortByDate = () => {
  console.log('yo = ');
  sortAsc.value = !sortAsc.value;
  sortedUsers.value.sort((a, b) => {
    const dateA = new Date(a.registered.date);
    const dateB = new Date(b.registered.date);
    return sortAsc.value ? dateA - dateB : dateB - dateA;
  });
};

const emit = defineEmits(['cardClicked']);

watch(() => props.userList, (newVal) => {
  if (newVal?.results) {
    sortedUsers.value = [...newVal.results];
    sortByDate();
  }
}, { immediate: true });
</script>

<template>

  <div class="">
    <div class="grid grid-cols-8 gap-4 px-8 text-gray-500 text-xs">
      <div class="cursor-pointer hover:underline flex items-center" @click="sortByDate()"> Date <img src="/updown.png"
          alt="" class="h-3 mr-2"></div>
      <div class="col-span-2">Name</div>
      <div class="">Gender</div>
      <div class="col-span-1 ">Country</div>
      <div class="">Email</div>
    </div>
    <div v-if="userList.results.length > 0">
      <div v-for="(user, index) in sortedUsers" :key="index" class="card mt-3" @click="emit('cardClicked', user)">
        <div
          class=" grid grid-cols-8 gap-4 shadow-md p-6 mb-4 rounded-md text-sm inset-ring inset-ring-gray-300/50 cursor-pointer">
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
