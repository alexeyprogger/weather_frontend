<template>
  <input
    :placeholder="placeholder"
    v-model="modelValue"
    @input="$emit('update:modelValue', modelValue)"
    class="custom-input"
  />
</template>

<script>
export default {
  name: "CustomInput",
  props: {
    placeholder: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      modelValue: null,
    };
  },
};
</script>

<style>
.custom-input {
  padding: 10px;
  margin-left: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 1rem;
}
</style>
