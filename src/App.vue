<template>
  <div class="app">
    <form class="input__form" @submit.prevent="getTimeDelta">
      <h1 class="input__form-title">Расписание поездов</h1>
      <input
        class="input__field input__from"
        type="text"
        placeholder="Откуда едем?"
        v-model="routes.departurePoint"
        required
      />
      <input
        class="input__field input__date-from"
        type="time"
        v-model="routes.departureTime"
      />
      <input
        class="input__field input__to"
        type="text"
        placeholder="Куда едем?"
        v-model="routes.arrivalPoint"
        required
      />
      <input
        class="input__field input__date-to"
        type="time"
        v-model="routes.arrivalTime"
      />
      <div class="input__field-btns">
        <button type="button" class="btn_add-route">Добавить пункт</button>
        <button type="button" class="btn_donload-routes">Мои маршруты</button>
      </div>
      <button class="btn_submit">Поехали!</button>
      <p v-if="routes.showRoute" class="route">
        {{ routes.departurePoint }} -> {{ routes.arrivalPoint }} | Время в пути:
        {{ routes.timeDifference }}
      </p>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      stations: ['Станция 1', 'Станция 2', 'Станция 3'],
      routes: [
        {
          id: "",
          departurePoint: null,
          arrivalPoint: null,
          departureTime: "00:00",
          arrivalTime: "00:00",
          showRoute: false,
          timeDifference: null,
          timeBetweenStops: null,
        },
      ],
      stopDuration: 5,
    };
  },
  methods: {
    getTimeDelta() {
      const [hoursFrom, minutesFrom] = this.routes.departureTime
        .split(":")
        .map(Number);
      const [hoursTo, minutesTo] = this.routes.arrivalTime.split(":").map(Number);

      const totalMinutesFrom = hoursFrom * 60 + minutesFrom;
      const totalMinutesTo = hoursTo * 60 + minutesTo;

      let timeDelta = totalMinutesTo - totalMinutesFrom;

      if (timeDelta < 0) {
        timeDelta += 24 * 60;
      }

      const deltaHours = Math.floor(timeDelta / 60);
      const deltaMinutes = timeDelta % 60;

      this.routes.timeDifference = `${String(deltaHours).padStart(2, "0")}:${String(
        deltaMinutes
      ).padStart(2, "0")}`;
      this.routes.showRoute = true;
    },
    createRoute() {},
    saveRoute() {},
    editeRoute() {},
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
  height: fit-content;
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

.input__field-btns {
  display: flex;
  justify-content: space-between;
}

.btn_donload-routes {
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

.btn_donload-routes:hover {
  background-color: rgb(119, 183, 243);
}

.btn_add-route {
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

.btn_add-route:hover {
  background-color: rgb(119, 183, 243);
}

.btn_submit {
  width: 200px;
  height: 40px;
  padding: 5px;
  border: none;
  align-self: center;
  background-color: #0095ff;
  border-radius: 5px;
  cursor: pointer;
  font-size: 18px;
  transition: background-color 0.3s ease;
}
.btn_submit:hover {
  background-color: rgb(119, 183, 243);
}

.route {
  font-size: 18px;
}
</style>
