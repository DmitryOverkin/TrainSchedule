<template>
  <input class="input__field input__from" type="text" :placeholder="placeholder" required />
  <input class="input__field input__date-from" type="time" :value="timeValue" />
</template>

<script>


export default {
    props:{
        placeholder: {
            type: String,
            required: false,
            default: 'Введите текст...'
        },
        timeValue: {
            type: String,
            required: true,
            default: '00:00'
        }
    }
};
</script>

<style scoped>
.input__field {
  padding: 10px;
  font-size: 18px;
  border-radius: 10px;
  border: none;
}
</style>
