<template>
  <div class="table-cell" :role="role">
    <slot> &nbsp; </slot>
  </div>
</template>

<script>
export default {
  name: "TableCell",
  props: {
    role: {
      default: "cell",
    },
  },
};
</script>

<style lang="scss" scoped></style>
