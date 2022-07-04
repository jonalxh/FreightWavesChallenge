<template>
	<div class="custom-table" role="table" :aria-label="title">
		<div class="table-header" role="rowgroup">
			<table-cell role="columnheader" class="text-uppercase" v-for="(field, fieldIndex) in fields" :key="fieldIndex">
				{{ field }}
			</table-cell>

			<table-cell role="columnheader" class="text-uppercase"> ACTIONS </table-cell>
		</div>

		<div class="table-body">
			<table-row :columns="fields" :data="item" v-for="(item, itemIndex) in items" :key="itemIndex" @row-click="$emit('row-click', item)">
				<template #action-buttons>
					<q-btn color="grey-8" icon="o_visibility" @click="$emit('row-click', item)" round flat>
						<q-tooltip>Details</q-tooltip>
					</q-btn>
					<q-btn color="grey-8" icon="o_edit" @click="$emit('row-edit', item)" round flat>
						<q-tooltip>Edit</q-tooltip>
					</q-btn>
				</template>
			</table-row>
		</div>
	</div>
</template>

<script>
import TableRow from "components/table/Row.vue";
import TableCell from "components/table/Cell.vue";

export default {
	props: {
		fields: {
			type: Array,
		},
		items: {
			type: Array,
			required: true,
		},
		title: {
			type: String,
			default: "Table",
		},
	},
	emits: ["row-edit", "row-click"],
	components: {
		TableCell,
		TableRow,
	},
};
</script>

<style lang="scss" scoped></style>
