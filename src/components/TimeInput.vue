<template>
  <input
    class="input__field input__date-from"
    type="time"
    :value="value"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: "00:00",
    },
  },
  methods: {},
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
