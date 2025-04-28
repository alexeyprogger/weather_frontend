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

<style lang="scss">
.custom-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  background-color: #3B82F6;
  &:hover {
    background-color: #2563EB;
  }
  color: white;
  font-size: 1rem;
}
</style>
