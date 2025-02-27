<template>
  <form class="input__form"><slot></slot></form>
</template>

<script>
export default {

};
</script>

<style scoped>
.input__form {
  width: 800px;
  height: fit-content;
  padding: 30px;
  gap: 20px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  background-color: #f2f4ff;
}
</style>
