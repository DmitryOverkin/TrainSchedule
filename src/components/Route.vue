<template>
  <p class="route">
    <span>Tver -> Moscow | Время в пути: 04:21</span>
    <span>
      <slot>
        <button class="route__btn save" type="button"></button>
        <button class="route__btn edit" type="button"></button>
      </slot>
    </span>
  </p>
</template>

<script>
export default {};
</script>

<style scoped>
.route {
  display: flex;
  justify-content: space-between;
  font-size: 18px;
}

.route__btn {
  width: 20px;
  height: 20px;
  border: none;
  border-radius: 5px;
  background: none;
  cursor: pointer;
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

.save {
  background-image: url("@/assets/saveIcon.svg");
}

.edit {
  background-image: url("@/assets/editIcon.svg");
  margin-left: 10px;
}
</style>
