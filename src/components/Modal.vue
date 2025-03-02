<template>
  <div class="modal">
    <div class="modal__window">
      <div class="modal__content">
        <div v-if="mySavedRoutes.length === 0">
          <h3 class="modal__content-text">
            У вас пока нет сохраненных маршрутов.
          </h3>
        </div>

        <div v-else v-for="(route, index) in mySavedRoutes">
          <div class="modal__route">
            <Route
              :route="route.route"
              :totalTime="route.totalTime"
              :stops="route.stops"
              :index="index"
              :isModalShow="isModalShow"
              @editRoute="$emit('editSavedRoute', index)"
              @deleteRoute="$emit('deleteSavedRoute', index)"
            />
          </div>
        </div>
        <div class="modal__btn">
          <Button @click="$emit('close')">Закрыть</Button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Route from "./Route.vue";
import Button from "./Button.vue";
export default {
  components: { Route, Button },
  props: {
    mySavedRoutes: {
      type: Array,
      default: () => [],
    },
    isModalShow: {
      type: Boolean,
    },
  },
  methods: {},
};
</script>

<style scoped>
.modal__window {
  width: 800px;
  height: fit-content;
  padding: 30px;
  border-radius: 10px;
  background-color: #f2f4ff;
}
.modal__route {
  margin-bottom: 30px;
}
.modal__content-text {
  text-align: center;
  align-self: center;
  margin-bottom: 50px;
}

.modal__btn {
  display: flex;
  justify-content: center;
}
</style>
