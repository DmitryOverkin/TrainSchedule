<template>
  <input
    class="input__field input__from"
    type="text"
    :placeholder="placeholder"
    required
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>

<script>
export default {
  props: {
    placeholder: {
      type: String,
      required: false,
      default: "Введите текст...",
    },
  },
};
</script>

<style scoped>
.input__field {
  padding: 10px;
  font-size: 18px;
  border-radius: 10px;
  border: none;
}

.input__date-from {
  margin-top: 20px;
}
</style>
