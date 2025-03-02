<template>
  <div class="modal__edit">
    <div class="modal__edit-window">
      <div class="modal__edit-content">
        <h3 class="modal__edit-title">Редактировать маршрут</h3>
        <div v-if="isEditing">
          <div
            class="input__form-points"
            v-for="(stop, index) in stops"
            :key="index"
          >
            <StationInput placeholder="Введите город" v-model="stop.station" />
            <TimeInput :value="stop.time" v-model="stop.time" />
          </div>
          <Button @click="saveChanges">Сохранить</Button>
          <!-- <div v-if="isEditing" v-for="(point, index) in points">
          <input v-model="localRoute.route" placeholder="Название маршрута" />
          <input v-model="localRoute.totalTime" placeholder="Общее время" />

          <div v-for="(stop, index) in localRoute.stops" :key="index">
            <input v-model="stop.station" placeholder="Станция" />
            <input v-model="stop.stopTime" placeholder="Время остановки" />
          </div>

     
        </div> -->
        </div>

        <div v-else class="modal__edit-btn">
          <Button @click="$emit('close')">Закрыть</Button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Button from "./Button.vue";
import StationInput from "./StationInput.vue";
import TimeInput from "./TimeInput.vue";
export default {
  components: {
    Button,
    StationInput,
    TimeInput,
  },
  props: {
    isEditing: Boolean,
    editedRoute: Object,
    points: Array,
  },
  data() {
    return {
      stops: [],
    };
  },
  methods: {
    convertRouteToStops() {
      const route = this.editedRoute.route;
      const routeParts = route.split(" -> ");
      this.stops = routeParts.map((part) => {
        const [station, time] = part.split(" (");
        return {
          station: station.trim(),
          time: time ? time.replace(")", "").trim() : "00:00",
        };
      });
    },

    calculateTimes() {
      let totalMinutes = 0;
      let prevTime = this.convertToMinutes(this.stops[0].time);
      let stops = [];

      for (let i = 1; i < this.stops.length; i++) {
        let currentTime = this.convertToMinutes(this.stops[i].time);
        if (currentTime < prevTime) {
          currentTime += 24 * 60;
        }
        let travelTime = currentTime - prevTime;

        if (i !== this.stops.length - 1) {
          travelTime += 5;
          stops.push({ station: this.stops[i].station, stopTime: "5 минут" });
        }

        totalMinutes += travelTime;
        prevTime = currentTime;
      }

      return {
        totalMinutes,
        stops,
      };
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

    saveChanges() {
      const updatedRoute = this.generateUpdatedRoute();
      this.$emit("saveEditedRoute", updatedRoute);
    },

    generateUpdatedRoute() {
      let updatedRouteString = this.stops[0].station;
      let prevTime = this.convertToMinutes(this.stops[0].time);
      let totalMinutes = 0;

      

      for (let i = 1; i < this.stops.length; i++) {
        let currentTime = this.convertToMinutes(this.stops[i].time);
        if (currentTime < prevTime) {
          currentTime += 24 * 60;
        }
        let travelTime = currentTime - prevTime;

        if (i !== this.stops.length - 1) {
          travelTime += 5; 
        }
        console.log(`i: ${i}, travelTime: ${travelTime}, totalMinutes: ${totalMinutes}`);

        updatedRouteString += ` -> ${this.stops[i].station} (${this.formatTime(
          travelTime
        )})`;

        totalMinutes += travelTime;
        prevTime = currentTime;
      }

      return {
        route: updatedRouteString,
        totalTime: this.formatTime(totalMinutes),
      };
    },
  },
  created() {
    this.convertRouteToStops();
  },
};
</script>

<style scoped>
.modal__edit-title {
  margin-bottom: 20px;
}

.modal__edit-window {
  width: 800px;
  height: fit-content;
  padding: 30px;
  border-radius: 10px;
  background-color: #f2f4ff;
}

.modal__edit-content {
  text-align: center;
}

.input__form-points {
  margin-bottom: 20px;
}

.modal__edit-btn {
  display: flex;
  justify-content: center;
}
</style>
