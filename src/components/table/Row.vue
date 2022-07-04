<template>
	<div class="table-row" role="rowgroup">
		<table-cell v-for="(field, fieldIndex) in columns" :key="fieldIndex" @click="$emit('row-click')">
			{{ data[field] }}
		</table-cell>

		<table-cell>
			<slot name="action-buttons">
				<!-- Custom slot to add buttons but keep the functionality in the parent  -->
			</slot>
		</table-cell>
	</div>
</template>

<script>
import { reactive, toRefs } from "vue";
import TableCell from "components/table/Cell.vue";

export default {
	name: "TableRow",
	emits: ["row-click"],
	props: {
		data: {
			type: Object,
			required: true,
		},
		columns: {
			type: Array,
		},
	},
	components: {
		TableCell,
	},
	setup(props) {
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
