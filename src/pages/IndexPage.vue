<template>
	<q-page class="q-pa-md">
		<div class="flex items-center justify-between q-mb-md">
			<div class="text-h5 text-bold">Users</div>

			<q-input v-model="filterText" label="Search users here" debounce="150" outlined>
				<template v-slot:prepend>
					<q-icon name="search" />
				</template>

				<template v-slot:append>
					<q-icon v-if="filterText" name="close" @click="filterText = null" class="cursor-pointer" />
				</template>
			</q-input>
		</div>

		<table-container :items="filteredUsers" :fields="['name', 'email', 'phone']" title="Users table" @row-click="showEditDialog" @row-edit="showEditDialog($event, false)" v-if="filteredUsers.length" />

		<div class="text-h5 text-center q-pa-md" v-else>
			{{ filterText ? "No users found" : "Loading users..." }}
		</div>
	</q-page>

	<q-dialog v-model="showDialog">
		<q-card>
			<q-card-section class="row items-center q-pb-none">
				<div class="text-h6">{{ readonly ? "User details" : "Edit user" }}</div>
				<q-space />
				<q-btn icon="close" flat round dense v-close-popup />
			</q-card-section>

			<q-card-section v-if="selectedUser">
				<q-form>
					<div class="row q-col-gutter-md">
						<!-- Here we iterate over the selectedUser fields to dynamically draw all inputs -->
						<template v-for="(fieldValue, fieldName, fieldIndex) in selectedUser" :key="fieldIndex">
							<q-input class="col-12" v-model="selectedUser[fieldName]" v-if="fieldName !== 'id' && ['number', 'string'].includes(typeof fieldValue)" :readonly="readonly" label-slot dense outlined>
								<template #label>
									<span class="text-capitalize">
										{{ fieldName }}
									</span>
								</template>
							</q-input>

							<template v-else-if="typeof fieldValue === 'object'">
								<!-- An alternative to create fields for objects  -->
								<div class="col-12 text-bold text-capitalize">{{ fieldName }}</div>

								<template v-for="(objValue, objName, objIndex) in fieldValue" :key="objIndex">
									<q-input class="col-12" v-model="selectedUser[fieldName][objName]" v-if="objName !== 'geo'" :readonly="readonly" label-slot dense outlined>
										<template #label>
											<span class="text-capitalize">
												{{ objName }}
											</span>
										</template>
									</q-input>
								</template>
							</template>
						</template>
					</div>
				</q-form>
			</q-card-section>

			<q-card-actions align="center" class="q-mb-md" v-if="!readonly">
				<q-btn label="Cancel" color="primary" flat v-close-popup />
				<q-btn label="Save" color="primary" @click="submitUserEdition" />
			</q-card-actions>
		</q-card>
	</q-dialog>
</template>

<script>
import { defineComponent, ref, onMounted, computed } from "vue";
import { useQuasar } from "quasar";
import { api } from "boot/axios";
import TableContainer from "src/components/table/TableContainer.vue";

export default defineComponent({
	name: "IndexPage",
	components: {
		TableContainer,
	},
	setup() {
		// Variables
		const $q = useQuasar();
		const filterText = ref(null);
		const readonly = ref(true);
		const selectedUser = ref(null);
		const showDialog = ref(false);
		const users = ref([]);

		// Computed variables
		const filteredUsers = computed(() => {
			if (filterText.value) {
				const text = filterText.value.toLowerCase();
				// Filters the users by the 3 selected details
				return users.value.filter((user) => user.name.toLowerCase().includes(text) || user.phone.includes(text) || user.email.toLowerCase().includes(text));
			} else {
				return users.value;
			}
		});

		// Methods
		const getUsers = () => {
			// Gets LocalStorage users or fetchs them from API
			const storedUsers = localStorage.getItem("users");

			if (storedUsers?.length) {
				users.value = JSON.parse(storedUsers);
			} else {
				api.get("users")
					.then((res) => {
						if (res.data) {
							users.value = res.data;
							localStorage.setItem("users", JSON.stringify(res.data)); // Saves initial users in LocalStorage
						}
					})
					.catch((error) => {
						$q.notify({
							message: error.stack,
							color: "negative",
							timeout: 5000,
							progress: true,
						});
					});
			}
		};

		const showEditDialog = (user, isReadonly = true) => {
			// isReadonly allows us to make the fields editable or readonly
			selectedUser.value = Object.assign({}, user); // We use Object.assign to create a non-reactive clone
			showDialog.value = true;
			readonly.value = isReadonly;
		};

		const submitUserEdition = () => {
			$q.loading.show();

			api.patch(`users/${selectedUser.value.id}`, selectedUser.value)
				.then((res) => {
					const userIndex = users.value.findIndex((user) => user.id === selectedUser.value.id);
					users.value[userIndex] = Object.assign({}, res.data);
					localStorage.setItem("users", JSON.stringify(users.value)); // We save the data again to keep it in memory
					showDialog.value = false;

					$q.notify({
						message: "User edited successfully",
						color: "positive",
						timeout: 5000,
						progress: true,
					});
				})
				.catch((error) => {
					$q.notify({
						message: error.stack,
						color: "negative",
						timeout: 5000,
						progress: true,
					});
				})
				.finally($q.loading.hide);
		};

		onMounted(() => {
			getUsers();
		});

		return {
			filteredUsers,
			filterText,
			readonly,
			selectedUser,
			showEditDialog,
			showDialog,
			submitUserEdition,
			users,
		};
	},
});
</script>

<style lang="scss" scoped></style>
