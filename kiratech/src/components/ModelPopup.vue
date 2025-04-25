<script setup>

const props = defineProps({
  card: Object,
  showPopup: Boolean,
});
const emit = defineEmits(['close']);

</script>

<template>
  <div v-if="showPopup" class="fixed inset-0 bg-gray-500/50 flex items-center justify-center z-50">
    <div class="bg-white rounded-2xl p-6 shadow-lg w-[90%] max-w-md text-center">
      <img src="/cross.png" alt="" class="ml-auto cursor-pointer" @click="emit('close')">
      <img :src="card.picture.medium" alt="Profile Picture" class="rounded-full mx-auto mb-4" />
      <h3 class="text-xl font-bold mb-4">{{ card.name.first }} {{ card.name.last }}</h3>
      <p class="mb-1">Email: <a :href="`mailto:${card.email}`" type="email" class="text-gray-500">{{ card.email
          }}</a></p>


      <p class="mb-1">Gender: {{ card.gender }}</p>
      <p class="mb-1">Country: {{ card.location.country }}</p>
      <p class="mb-4">Phone: {{ card.phone }}</p>
    </div>
  </div>
</template>

<style scoped>
</style>
