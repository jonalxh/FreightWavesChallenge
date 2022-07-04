<template>
	<div class="table-cell" :role="role">
		<slot> &nbsp; </slot>
	</div>
</template>

<script>
import { reactive, toRefs } from "vue";

export default {
	name: "TableCell",
	props: {
		role: {
			default: "cell",
		},
	},
	setup() {
		const state = reactive({
			count: 0,
		});

		return {
			...toRefs(state),
		};
	},
};
</script>

<style lang="scss" scoped></style>
