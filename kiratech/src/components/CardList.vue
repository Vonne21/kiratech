<script setup>
import { ref, watch, computed } from 'vue'

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

const sortOrder = ref('default');

const sortedUsers = computed(() => {
  if (sortOrder.value === 'default') return props.userList.results;
  return [...props.userList.results].sort((a, b) => {
    const dateA = new Date(a.registered.date);
    const dateB = new Date(b.registered.date);
    return sortOrder.value === 'asc' ? dateA - dateB : dateB - dateA;
  });
});

const sortByDate = () => {
  sortOrder.value = sortOrder.value === 'default' ? 'asc' : sortOrder.value === 'asc' ? 'desc' : 'default';
};

const emit = defineEmits(['cardClicked']);

watch(() => props.userList, (newVal) => {
    if (newVal?.results) {
      sortOrder.value = 'default';
    }
  }, { immediate: true });
</script>

<template>

  <div class="mx-5">
    <div class="hidden md:grid grid-cols-8 gap-4 px-8 text-gray-500 text-xs">
      <div class="cursor-pointer hover:underline flex items-center" @click="sortByDate()"> Date <img
          v-if="sortOrder === 'default'" src="/sort-default.png" alt="Default" class="h-3" />
        <img v-else-if="sortOrder === 'asc'" src="/sort-asc.png" alt="Asc" class="h-3" />
        <img v-else src="/sort-desc.png" alt="Desc" class="h-3" />
      </div>
      <p class="col-span-2">Name</p>
      <p class="">Gender</p>
      <p class="col-span-1 ">Country</p>
      <p class="">Email</p>
    </div>
    <div v-if="userList.results.length > 0">
      <div v-for="(user, index) in sortedUsers" :key="index" class="card mt-3" @click="emit('cardClicked', user)">
        <div
          class="group grid grid-cols-1 lg:grid-cols-8 gap-y-2 gap-x-4 shadow-md p-6 mb-4 rounded-md text-sm inset-ring inset-ring-gray-300/50 cursor-pointer hover:ring-2 ring-cyan-500">

          <p class="col-span-1 text-gray-500 break-words font-semibold lg:font-normal">
            {{ formatDate(user.registered.date) }}</p>

          <div class="lg:col-span-2 flex lg:block">
            <span class="lg:hidden w-24 shrink-0 text-xs font-normal text-gray-400">Name </span>
            <span class="font-semibold group-hover:text-cyan-500 break-words">
              {{ user.name.first }} {{ user.name.last }}
            </span>
          </div>

          <div class="lg:col-span-1 flex lg:block text-black lg:text-gray-500 group-hover:text-black">
            <span class="lg:hidden w-24 shrink-0 text-xs text-gray-400">Gender </span>
            <span>{{ user.gender }}</span>
          </div>

          <div class="lg:col-span-1 flex lg:block ">
            <span class="lg:hidden w-24 shrink-0 text-xs text-gray-400">Country </span>
            <span class="break-words">{{ user.location.country }}</span>
          </div>

          <div class="lg:col-span-3 flex lg:block text-black lg:text-gray-500 break-all">
            <span class="lg:hidden w-24 shrink-0 text-xs text-gray-400">Email </span>
            <span class="break-words">{{ user.email }}</span>
          </div>
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
