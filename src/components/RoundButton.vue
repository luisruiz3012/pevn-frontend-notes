<template>
  <button @click="openModal">+</button>
</template>

<script>
export default {
  name: "RoundButton",
  methods: {
    openModal() {
      this.$emit("openModal");
    },
  },
};
</script>

<style scoped>
button {
  background-color: aquamarine;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  border: 1px solid rgb(0, 194, 129);
  position: fixed;
  bottom: 40px;
  right: 40px;
  font-size: 28px;
  color: rgb(3, 170, 114);
}
</style>
