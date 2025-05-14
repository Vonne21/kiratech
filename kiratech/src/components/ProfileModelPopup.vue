<script setup>

const props = defineProps({
  card: Object,
  showPopup: Boolean,
});
const emit = defineEmits(['close']);
const formatDate = (rawDate) => {
  const date = new Date(rawDate);
  const day = date.getDate();
  const month = date.toLocaleString('default', { month: 'short' });
  const year = date.getFullYear();
  return `${day} ${month} ${year}`;
};
</script>

<template>
  <Transition name="fade">
    <div v-if="showPopup" class="fixed inset-0 bg-gray-500/50 flex items-center justify-center z-50">
      <div class="bg-white rounded-2xl p-6 shadow-lg w-[90%] max-w-md text-center">
        <img src="/cross.png" alt="" class="ml-auto cursor-pointer" @click="emit('close')">
        <img :src="card.picture.medium" alt="Profile Picture" class="rounded-full mx-auto mb-4" />
        <h3 class="text-xl font-bold mb-4">{{ card.name.first }} {{ card.name.last }}</h3>
        <div class="text-left mx-auto w-full">

          <div class="col-span-2 flex gap-2 mb-2">
            <span class="text-gray-500 w-24 shrink-0">Date:</span>
            <p class="mb-1 break-words flex-1">{{ formatDate(card.registered.date) }}</p>
          </div>

          <div class="col-span-2 flex gap-2 mb-2">
            <span class="text-gray-500 w-24 shrink-0">Phone:</span>
            <p class="mb-1 break-words flex-1">{{ card.phone }}</p>
          </div>

          <div class="col-span-2 flex gap-2 mb-2">
            <span class="text-gray-500 w-24 shrink-0">Gender:</span>
            <p class="mb-1 flex-1">{{ card.gender }}</p>
          </div>

          <div class="col-span-2 flex gap-2 mb-2">
            <span class="text-gray-500 w-24 shrink-0">Country:</span>
            <p class="mb-1 break-words flex-1">{{ card.location.country }}</p>
          </div>
          
          <div class="col-span-2 flex gap-2">
            <span class="text-gray-500 w-24 shrink-0">Email:</span>
            <p class="mb-1 break-words break-all flex-1">
              <a :href="`mailto:${card.email}`" type="email">{{ card.email }}</a>
            </p>
          </div>

        </div>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
