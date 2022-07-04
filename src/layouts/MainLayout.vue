<template>
	<q-layout view="hHh lpR fFf">
		<q-header elevated>
			<q-toolbar>
				<q-btn icon="menu" aria-label="Menu" @click="toggleLeftDrawer" flat dense round />

				<q-toolbar-title> FreightWaves Challenge </q-toolbar-title>
			</q-toolbar>
		</q-header>

		<q-drawer v-model="leftDrawerOpen" behavior="mobile" elevated>
			<q-list>
				<q-item-label header> Main menu </q-item-label>

				<EssentialLink v-for="link in essentialLinks" :key="link.title" v-bind="link" />
			</q-list>
		</q-drawer>

		<q-page-container>
			<router-view />
		</q-page-container>
	</q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import EssentialLink from "components/EssentialLink.vue";

const linksList = [
	{
		title: "Home",
		icon: "home",
		link: "/",
	},
];

export default defineComponent({
	name: "MainLayout",

	components: {
		EssentialLink,
	},

	setup() {
		const leftDrawerOpen = ref(false);

		const toggleLeftDrawer = () => {
			leftDrawerOpen.value = !leftDrawerOpen.value;
		};

		return {
			essentialLinks: linksList,
			leftDrawerOpen,
			toggleLeftDrawer,
		};
	},
});
</script>
