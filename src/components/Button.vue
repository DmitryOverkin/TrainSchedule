<template>
  <button class="btn" :type="buttonType"><slot></slot></button>
</template>

<script>
export default {
  props: {
    buttonType: {
      type: String,
      default: "button",
    },
  },
};
</script>

<style scoped>
.btn {
  width: 200px;
  height: 40px;
  padding: 5px;
  border: none;
  align-self: flex-start;
  background-color: #0095ff;
  border-radius: 5px;
  cursor: pointer;
  font-size: 18px;
  transition: background-color 0.3s ease;
}

button[type="submit"] {
  margin: 0 auto;
}

.btn:hover {
  background-color: rgb(119, 183, 243);
}
</style>
