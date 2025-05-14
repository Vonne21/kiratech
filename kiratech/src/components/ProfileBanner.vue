<script setup>
import { ref } from 'vue'
import GenericModel from './GenericModel.vue'

const modalType = ref('')
const showModal = ref(false)

function showGenericModel(type) {
	modalType.value = type
	showModal.value = true
}
function closeGenericModel() {
	showModal.value = false
}
</script>

<template>
	<div class="bg-cyan-500 w-full min-h-[154px]">
		<div class="max-w-5xl mx-auto flex flex-col justify-center content-center md:flex-row">
			<img src="/Avatar.png" alt="" class="w-36 h-36 self-center md:relative md:top-14 mt-3 md:mt-0">
			<div class="flex flex-col text-white self-center md:self-end text-center md:text-left  mb-2 ml-0 md:ml-5">
				<p class="font-bold text-2xl">John Doee</p>
				<p>Last Online: 2 days ago</p>
			</div>
			<div class="flex flex-row self-center md:self-end ml-0 md:ml-4 mb-2">
				<button
					class="bg-white px-3 py-2 text-xs font-semibold text-cyan-500 rounded-sm h-fit flex mx-2 items-center cursor-pointer min-w-[130px] justify-center"
					@click="showGenericModel('send')">
					<img src="/send.png" alt="" class="h-3 mr-2">
					<p>Send Message</p>
				</button>
				<button
					class="outline outline-white px-3 py-2 text-xs font-semibold text-white rounded-sm h-fit flex items-center cursor-pointer min-w-[130px] justify-center"
					@click="showGenericModel('add')">
					<img src="/add.png" alt="" class="h-3 mr-2">
					<p>Add Friend</p>
				</button>
			</div>
		</div>
	</div>

	<GenericModel :show="showModal" @closeGenericModel="closeGenericModel">
		<template #header>
			<h2 class="text-xl font-bold">
				{{ modalType === 'send' ? 'Send Message' : 'Add Friend' }}
			</h2>
		</template>

		<template #default>
			<p v-if="modalType === 'send'">Send a message to your friend 🎯</p>
			<p v-else-if="modalType === 'add'">Add a new buddy to your network 🤝</p>
		</template>

		<template #footer>
			<button class="bg-cyan-500 text-white px-4 py-2 rounded" @click="closeGenericModel">OK</button>
		</template>
	</GenericModel>

</template>