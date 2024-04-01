<template>
  <div class="info">
    <div class="info-topPuntuacion">
      <div class="info-topPuntuacion-topScore">
        <span>Top Score: {{ record }}</span>
      </div>
    </div>
    <div class="info-puntuacion">
      <div class="info-puntuacion-puntos">
        <span>Puntación: {{ puntos }}</span>
      </div>
      <div class="info-puntuacion-newGame">
        <button
          @click="
            newGame();
            guardarRecord(puntos);
          "
          class="btn btn-newGame"
        >
          New Game
        </button>
      </div>
    </div>
  </div>
</template>
<script setup>
import { defineProps, onMounted, ref } from "vue";

const { puntos, newGame } = defineProps(["puntos", "newGame"]);

const record = ref(0);

const guardarRecord = (puntos) => {
  const actualRecord = localStorage.getItem("recordPuntuacion") || 0;
  if (puntos > actualRecord) {
    localStorage.setItem("recordPuntuacion", puntos);
    record.value = puntos;
  }
};

const getRecord = () => {
  return localStorage.getItem("recordPuntuacion") || 0;
};

onMounted(() => {
  record.value = getRecord();
});
</script>
<style></style>
