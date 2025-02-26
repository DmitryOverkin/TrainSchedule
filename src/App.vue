<template>
  <div class="app">
    <form  class="input__form" @submit.prevent="getTimeDelta">
      <h1 class="input__form-title">Расписание поездов</h1>
      <input
        class="input__field input__from"
        type="text"
        placeholder="Откуда едем?"
        v-model="departurePoint"
        required
      />
      <input
        class="input__field input__date-from"
        type="time"
        v-model="departureTime"
      />
      <input
        class="input__field input__to"
        type="text"
        placeholder="Куда едем?"
        v-model="arrivalPoint"
        required
      />
      <input class="input__field input__date-to" type="time" v-model="arrivalTime" />

      <button type="button" class="btn_add-route">Добавить точку</button>
      <button  class="btn_submit" >Поехали!</button>
      <p v-if="showRoute" class="route">{{ departurePoint }} -> {{ arrivalPoint }} | Время в пути: {{ timeDifference }}</p>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      departurePoint: null,
      arrivalPoint: null,
      departureTime: "00:00",
      arrivalTime: "00:00",
      timeDifference: null,
      stopDuration: 5,
      timeBetweenStops: null,
      showRoute: false
    };
  },
  methods: {
    getTimeDelta() {
      const [hoursFrom, minutesFrom] = this.departureTime.split(":").map(Number);
      const [hoursTo, minutesTo] = this.arrivalTime.split(":").map(Number);

      const totalMinutesFrom = hoursFrom * 60 + minutesFrom;
      const totalMinutesTo = hoursTo * 60 + minutesTo;

      let timeDelta = totalMinutesTo - totalMinutesFrom;

      if (timeDelta < 0) {
        timeDelta += 24 * 60;
      }

      const deltaHours = Math.floor(timeDelta / 60);
      const deltaMinutes = timeDelta % 60;

      this.timeDifference = `${String(deltaHours).padStart(2, "0")}:${String(
        deltaMinutes
      ).padStart(2, "0")}`;
      this.showRoute = true
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Rubik", serif;
  font-weight: 500;
  font-style: normal;
}
.app {
  width: 100vw;
  height: 100vh;
  display: flex;
  background-color: #6c756b;
  align-items: center;
  justify-content: center;
}

.input__form {
  width: 800px;
  height: 600px;
  padding: 30px;
  gap: 20px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  background-color: #f2f4ff;
}

.input__field {
  padding: 10px;
  font-size: 18px;
  border-radius: 10px;
  border: none;
}

.input__form-title {
  text-align: center;
}

.btn_add-route {
  width: 200px;
  height: 40px;
  padding: 5px;
  border: none;
  align-self: flex-start;
  background-color: #0095ff;
  cursor: pointer;
  font-size: 18px;
  transition: background-color 0.3s ease;
}

.btn_submit {
  width: 200px;
  height: 40px;
  padding: 5px;
  border: none;
  align-self: center;
  background-color: #0095ff;
  cursor: pointer;
  font-size: 18px;
  transition: background-color 0.3s ease;
}
.btn_submit:hover {
  background-color: rgb(119, 183, 243);
}

.route{
  font-size: 18px;
}
</style>
