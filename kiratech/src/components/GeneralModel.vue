<script setup>
import {computed } from 'vue'

const props = defineProps({
  typeOfModel: String,
  showGeneralModel: Boolean,
});
const emit = defineEmits(['closeGeneralModel']);
const msg = computed(() => {
  switch (props.typeOfModel) {
    case 'send':
      return 'Send Message to your friend!';
    case 'add':
      return 'Add new friend!';
    default:
      return 'No message available.';
  }
});

</script>

<template>
  <div v-if="showGeneralModel" class="fixed inset-0 bg-gray-500/50 flex items-center justify-center z-50">
    <div class="bg-white rounded-2xl p-6 shadow-lg w-[90%] max-w-md text-center mb-3">
      <img src="/cross.png" alt="" class="ml-auto cursor-pointer" @click="emit('closeGeneralModel')">
      <p>{{ msg }}</p>
    </div>
    </div>
</template>

<style scoped>
</style>
