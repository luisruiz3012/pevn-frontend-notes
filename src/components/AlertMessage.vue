<template>
  <div
    class="alert alert-dismissible fade show m-4 pb-0 pt-4 alertMessage"
    v-bind:class="alert.status == 'success' ? 'alert-success' : 'alert-danger'"
  >
    <p>{{ alert.message }}</p>
    <button
      type="button"
      class="close mt-2"
      data-dismiss="alert"
      aria-label="Close"
    >
      <span class="pt-4" aria-hidden="true">&times;</span>
    </button>
  </div>
</template>

<script>
export default {
  name: "AlertMessage",
  props: {
    alert: {},
  },
};
</script>

<style scoped>
.alertMessage {
  position: relative;
  top: 0px;
}
</style>
