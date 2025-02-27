<template>
  <div class="app">
    <InputForm class="input__form">
      <h1 class="input__form-title">Расписание поездов</h1>
      <StationInput placeholder="Откуда едем?" />
      <StationInput placeholder="Куда едем?" />
      <div class="input__field-btns">
        <Button>Добавить пункт</Button>
        <Button>Мои маршруты</Button>
      </div>
      <Button type="submit">Поехали!</Button>
      <Route> </Route>
    </InputForm>
  </div>
</template>

<script>
import InputForm from "./components/InputForm.vue";
import StationInput from "./components/StationInput.vue";
import Button from "./components/Button.vue";
import Route from "./components/Route.vue";

export default {
  components: {
    InputForm,
    StationInput,
    Button,
    Route,
  },
  data() {
    return {
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
      // const [hoursFrom, minutesFrom] = this.routes.departureTime
      //   .split(":")
      //   .map(Number);
      // const [hoursTo, minutesTo] = this.routes.arrivalTime
      //   .split(":")
      //   .map(Number);

      // const totalMinutesFrom = hoursFrom * 60 + minutesFrom;
      // const totalMinutesTo = hoursTo * 60 + minutesTo;

      // let timeDelta = totalMinutesTo - totalMinutesFrom;

      // if (timeDelta < 0) {
      //   timeDelta += 24 * 60;
      // }

      // const deltaHours = Math.floor(timeDelta / 60);
      // const deltaMinutes = timeDelta % 60;

      // this.routes.timeDifference = `${String(deltaHours).padStart(
      //   2,
      //   "0"
      // )}:${String(deltaMinutes).padStart(2, "0")}`;
      // this.routes.showRoute = true;
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

.input__form-title {
  text-align: center;
}

.input__field-btns {
  display: flex;
  justify-content: space-between;
}
</style>
