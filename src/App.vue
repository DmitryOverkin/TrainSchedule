<template>
  <div class="app">
    <div v-if="isModalShow">
      <Modal
        @close="isModalShow = false"
        @deleteSavedRoute="deleteSavedRoute"
        :mySavedRoutes="mySavedRoutes"
      />
    </div>
    <div v-else-if="isEditing">
      <EditModal
        @close="isEditing = false"
        :isEditing="isEditing"
        :editedRoute="editedRoute"
        @saveEditedRoute="saveEditedRoute"
        :points="points"
      />
    </div>
    <InputForm v-else class="input__form" @submit.prevent>
      <h1 class="input__form-title">Расписание поездов</h1>
      <div
        class="input__form-points"
        v-for="(point, index) in points"
        :key="forceUpdateKey + '-' + index"
      >
        <StationInput placeholder="Введите город" v-model="point.point" />
        <TimeInput :value="point.time" v-model="point.time" />
      </div>
      <div class="input__field-btns">
        <Button @click="addPoint">Добавить пункт</Button>
        <Button @click="isModalShow = true">Мои маршруты</Button>
      </div>
      <Button type="submit" @click="createRoute">Поехали!</Button>
      <div v-for="(route, index) in routes" :key="index">
        <Route
          :route="route.route"
          :totalTime="route.totalTime"
          :stops="route.stops"
          :index="index"
          @editRoute="editeRoute"
          @saveRoute="saveRoutes(index)"
          @deleteRoute="deleteRoute(index)"
        />
      </div>
    </InputForm>
  </div>
</template>

<script>
import InputForm from "./components/InputForm.vue";
import StationInput from "./components/StationInput.vue";
import Button from "./components/Button.vue";
import Route from "./components/Route.vue";
import TimeInput from "./components/TimeInput.vue";
import Modal from "./components/Modal.vue";
import EditModal from "./components/EditModal.vue";

export default {
  components: {
    InputForm,
    StationInput,
    Button,
    Route,
    TimeInput,
    Modal,
    EditModal,
  },
  data() {
    return {
      points: [
        { point: "", time: "00:00" },
        { point: "", time: "00:00" },
      ],
      routes: [],
      mySavedRoutes: [],
      stopDuration: 5,
      isModalShow: false,
      forceUpdateKey: 0,
      isModalShow: false,
      isEditing: false,
      editedRoute: { route: "", totalTime: "", stops: [] },
      editedRouteIndex: null,
    };
  },
  methods: {
    addPoint() {
      this.points.push({
        point: this.points.point,
        time: this.points.time,
      });
    },
    createRoute() {
      if (this.points.length < 2) {
        alert("Недостаточно данных для построения маршрута");
        return;
      }

      for (let i = 0; i < this.points.length; i++) {
        if (!this.points[i].point.trim()) {
          alert(`Введите название города для пункта ${i + 1}`);
          return;
        }
      }

      let route = `${this.points[0].point}`;
      let totalMinutes = 0;
      let prevTime = this.convertToMinutes(this.points[0].time);
      let stops = [];

      for (let i = 1; i < this.points.length; i++) {
        let currentTime = this.convertToMinutes(this.points[i].time);
        if (currentTime < prevTime) {
          currentTime += 24 * 60;
        }
        let travelTime = currentTime - prevTime;

        if (i !== this.points.length - 1) {
          travelTime += this.stopDuration;
          stops.push({ station: this.points[i].point, stopTime: "5 минут" });
        }

        route += ` -> ${this.points[i].point} (${this.formatTime(travelTime)})`;
        totalMinutes += travelTime;
        prevTime = currentTime;
      }

      let newRoute = {
        route: route,
        totalTime: this.formatTime(totalMinutes),
        stops: stops,
      };

      this.routes.push(newRoute);

      this.points = [
        { point: "", time: "00:00" },
        { point: "", time: "00:00" },
      ];

      this.forceUpdateKey++;
    },
    convertToMinutes(time) {
      let [hours, minutes] = time.split(":").map(Number);
      return hours * 60 + minutes;
    },
    formatTime(minutes) {
      let hours = Math.floor(minutes / 60);
      let mins = minutes % 60;
      return `${hours}ч ${mins}м`;
    },
    saveRoutes(index) {
      this.mySavedRoutes.push(this.routes[index]);
      localStorage.setItem("savedRoutes", JSON.stringify(this.mySavedRoutes));
      alert("маршрут сохранен");
    },
    loadRoutes() {
      const savedRoutes = localStorage.getItem("savedRoutes");
      if (savedRoutes) {
        this.mySavedRoutes = JSON.parse(savedRoutes);
      }
    },

    editeRoute(index) {
      this.isEditing = true;
      this.editedRouteIndex = index;
      this.editedRoute = { ...this.routes[index] };
    },

    saveEditedRoute(updatedRoute) {
      if (this.editedRouteIndex !== null) {
        this.routes[this.editedRouteIndex].route = updatedRoute.route;
        this.routes[this.editedRouteIndex].totalTime = updatedRoute.totalTime;
      }
      this.isEditing = false;
      this.editedRouteIndex = null;
    },

    deleteRoute(index) {
      this.routes.splice(index, 1);
      localStorage.setItem("savedRoutes", JSON.stringify(this.routes));
    },
    deleteSavedRoute(index) {
      this.mySavedRoutes.splice(index, 1);
      localStorage.setItem("savedRoutes", JSON.stringify(this.mySavedRoutes));
    },
  },
  created() {
    this.loadRoutes();
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
  width: 100%;
  height: 100%;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #6c756b;
}

.input__form-title {
  text-align: center;
}

.input__form-points {
  display: flex;
  flex-direction: column;
}
.input__field-btns {
  display: flex;
  justify-content: space-between;
}
</style>
