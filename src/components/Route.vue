<template>
  <p class="route">
    <span class="route__title">{{ route }}</span>
    <span class="route__stop-station-time">
      <span v-if="stopStation">{{ stopStation }}</span>
      <span v-if="stopTime">: <br />{{ stopTime }}</span>
    </span>
    <span class="route__time"> {{ totalTime }}</span>
    <span>
      <slot>
        <div class="route__btns">
          <button class="route__btn save" type="button"></button>
          <button class="route__btn edit" type="button"></button>
        </div>
      </slot>
    </span>
  </p>
</template>

<script>
export default {
  props: {
    route: {
      type: String,
      default: "",
    },
    totalTime: {
      type: String,
      default: null,
    },
    stopTime: {
      type: String,
      required: false,
      default: null,
    },
    stopStation: {
      type: String,
      required: false,
      default: "",
    },
  },
};
</script>

<style scoped>
.route {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  grid-template-rows: 1fr;
  grid-column-gap: 15px;
  grid-row-gap: 10px;
  font-size: 18px;
}

.route__title {
  grid-area: 1 / 1 / 2 / 4;
}

.route__stop-station-time {
  grid-area: 1 / 4 / 2 / 5;
  text-align: right;
}

.route__time {
  grid-area: 1 / 5 / 2 / 6;
  text-align: right;
}

.route__btns {
  grid-area: 1 / 6 / 2 / 7;
  display: flex;
  justify-content: flex-end;
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
