<script setup>
const props = defineProps({
  show: Boolean,
  closable: {
    type: Boolean,
    default: true,
  },
})
const emit = defineEmits(['closeGenericModel'])

</script>

<template>
  <Transition name="fade">
    <div v-if="props.show" class="fixed inset-0 bg-gray-500/50 flex items-center justify-center z-50">
      <div class="bg-white rounded-2xl p-6 shadow-lg w-[90%] max-w-md text-center mb-3" @click.stop>
        <div class="flex items-center mb-6 pb-3 border-b border-cyan-500">
          <slot name="header">
            <h2 class="text-lg font-semibold">Modal</h2>
          </slot>
          <img v-if="props.closable" src="/cross.png" alt="close" class="ml-auto  w-6 h-6 cursor-pointer"
            @click="emit('closeGenericModel')" />
        </div>

        <div class="mb-6 text-center">
          <slot />
        </div>

        <div class="text-center">
          <slot name="footer" />
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
