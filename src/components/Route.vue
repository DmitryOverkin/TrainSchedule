<template>
  <div class="route">
    <h4 class="route__title">{{ route }}</h4>

    <div class="route__stop-station-time">
      <span v-if="stops.length > 0">Время стоянки:</span><br /><br />
      <span v-for="(stop, index) in stops" :key="index">
        <span>{{ stop.station }}</span
        >:<br />
        <span>{{ stop.stopTime }}</span
        ><br /><br />
      </span>
    </div>

    <div class="route__time">{{ totalTime }}</div>

    <div>
      <slot>
        <div class="route__btns">
          <button
            class="route__btn save"
            type="button"
            @click="$emit('saveRoute')"
          ></button>
          <button
            class="route__btn edit"
            type="button"
            @click="$emit('editRoute', index)"
          ></button>
          <button
            class="route__btn delete"
            type="button"
            @click="$emit('deleteRoute')"
          ></button>
        </div>
      </slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    route: {
      type: String,
      required: true,
      default: "",
    },
    totalTime: {
      type: String,
      default: null,
    },
    index: Number,
    stops: {
      type: Array,
      default: () => [],
    },
  },
  methods: {},
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
  border-bottom: 1px solid;
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
  margin: 0 10px;
}

.delete {
  background-image: url("@/assets/deleteIcon.svg");
}
</style>
