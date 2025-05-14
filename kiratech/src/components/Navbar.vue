<script setup>
import { ref, computed } from 'vue'
import GenericModel from './GenericModel.vue'

const modalType = ref('')
const showModal = ref(false)

function showGenericModel(type) {
	modalType.value = type;
	showModal.value = true;
}
function closeGenericModel() {
	showModal.value = false;
}

const modalTitle = computed(() => {
	switch (modalType.value) {
		case 'noti':
			return 'Notifications';
		case 'setting':
			return 'Setting';
		case 'logout':
			return 'Logout';
		default:
			return 'Modal';
	}
});
const modalContent = computed(() => {
	switch (modalType.value) {
		case 'noti':
			return 'Your Notifications 🔔';
		case 'setting':
			return 'Setting 🤖';
		case 'logout':
			return 'Are you sure you want to logout?';
		default:
			return 'No message available.';
	}
});


</script>
<template>
	<nav class="top-0 left-0 z-50 w-full py-4">
		<div class="flex max-w-5xl mx-auto">
			<img src="/Logo.png" alt="" class="max-w-30 lg:max-w-44 mr-auto ml-7" />
			<div class="flex items-center mr-5 lg:mr-7">
				<img src="/notifications.png" alt="" class="w-11 h-11 cursor-pointer"
					@click="showGenericModel('noti')" />
				<img src=" /settings.png" alt="" class="w-11 h-11 cursor-pointer"
					@click="showGenericModel('setting')" />
				<img src="/logout.png" alt="" class="w-11 h-11 cursor-pointer" @click="showGenericModel('logout')" />
			</div>

		</div>
	</nav>

	<GenericModel :show="showModal" @closeGenericModel="closeGenericModel">
		<template #header>
			<h2 class="text-xl font-bold hrllo">
				{{ modalTitle }}
			</h2>
		</template>

		<template #default>
			{{ modalContent }}
		</template>

		<template #footer>
			<button class="bg-cyan-500 text-white px-4 py-2 rounded" @click="closeGenericModel">OK</button>
		</template>
	</GenericModel>
</template>
