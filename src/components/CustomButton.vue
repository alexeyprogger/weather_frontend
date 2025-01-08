<template>
  <button class="custom-button" @click="onClick">
    <slot />
  </button>
</template>

<script>
export default {
  name: "CustomButton",
  emits: ["click"],
  methods: {
    onClick() {
      this.$emit("click");
    },
  },
};
</script>

<style>
.custom-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  font-size: 1rem;
}
</style>
