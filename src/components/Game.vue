<template>
  <main @keydown="handleKeyDown" tabindex="0" class="main">
    <Puntuacion :puntos="puntos" :newGame="newGame" />
    <div ref="parent" class="grid" id="grid">
      <div v-for="(fila, indexFila) in gridState" :key="indexFila" class="fila">
        <div
          v-for="(celda, indexCelda) in fila"
          :key="indexCelda"
          class="celda"
        >
          <span v-if="celda !== 0" :class="['animate__animated', 'animate__fadeInUp']" :style="getStyle(celda)">{{ celda }}</span>
          <span v-else :style="getStyle(celda)"></span>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { onMounted, ref } from "vue";
import Puntuacion from "./Puntuacion.vue";
import { useAutoAnimate } from '@formkit/auto-animate/vue';

const [parent] = useAutoAnimate();

const emit = defineEmits(["newGame"]);

const gridState = ref([
  [0, 0, 0, 0],
  [0, 0, 0, 0],
  [0, 0, 0, 0],
  [0, 0, 0, 0],
]);

let puntos = ref(0);

const detectarCeldasVacias = () => {
  const celdasVacias = [];
  for (let i = 0; i < gridState.value.length; i++) {
    for (let j = 0; j < gridState.value[i].length; j++) {
      if (gridState.value[i][j] === 0) {
        celdasVacias.push({ fila: i, columna: j });
      }
    }
  }
  return celdasVacias;
};

const generarNumero = (moved) => {
  if (moved) {
    const celdasVacias = detectarCeldasVacias();
    const randomNumber = Math.random() < 0.8 ? 2 : 4;
    const celdaVacia = Math.floor(Math.random() * celdasVacias.length);
    const { fila, columna } = celdasVacias[celdaVacia];
    gridState.value[fila][columna] = randomNumber;
  }
};

function moverArriba() {
  let moved = false;
  for (let fila = 0; fila < gridState.value[0].length; fila++) {
    for (let columna = 1; columna < gridState.value.length; columna++) {
      if (gridState.value[columna][fila] !== 0) {
        while (columna > 0 && gridState.value[columna - 1][fila] === 0) {
          gridState.value[columna - 1][fila] = gridState.value[columna][fila];
          gridState.value[columna][fila] = 0;
          columna--;
          moved = true;
        }
        if (
          columna > 0 &&
          gridState.value[columna - 1][fila] === gridState.value[columna][fila]
        ) {
          const num = (gridState.value[columna - 1][fila] *= 2);
          gridState.value[columna][fila] = 0;
          moved = true;
          puntos.value += num;
        }
      }
    }
  }
  generarNumero(moved);
}

const moverAbajo = () => {
  let moved = false;
  for (let fila = 0; fila < gridState.value[0].length; fila++) {
    for (let columna = gridState.value.length - 2; columna >= 0; columna--) {
      if (gridState.value[columna][fila] !== 0) {
        while (
          columna < gridState.value.length - 1 &&
          gridState.value[columna + 1][fila] === 0
        ) {
          gridState.value[columna + 1][fila] = gridState.value[columna][fila];
          gridState.value[columna][fila] = 0;
          columna++;
          moved = true;
        }
        if (
          columna < gridState.value.length - 1 &&
          gridState.value[columna + 1][fila] === gridState.value[columna][fila]
        ) {
          const num = (gridState.value[columna + 1][fila] *= 2);
          gridState.value[columna][fila] = 0;
          moved = true;
          puntos.value += num;
        }
      }
    }
  }
  generarNumero(moved);
};
const moverIzquierda = () => {
  let moved = false;
  for (let fila = 0; fila < gridState.value.length; fila++) {
    for (let columna = 0; columna < gridState.value[0].length; columna++) {
      if (gridState.value[fila][columna] !== 0) {
        while (columna > 0 && gridState.value[fila][columna - 1] === 0) {
          gridState.value[fila][columna - 1] = gridState.value[fila][columna];
          gridState.value[fila][columna] = 0;
          columna--;
          moved = true;
        }
        if (
          columna > 0 &&
          gridState.value[fila][columna - 1] === gridState.value[fila][columna]
        ) {
          const num = (gridState.value[fila][columna - 1] *= 2);
          gridState.value[fila][columna] = 0;
          moved = true;
          puntos.value += num;
        }
      }
    }
  }
  generarNumero(moved);
};

const moverDerecha = () => {
  let moved = false;
  for (let fila = 0; fila < gridState.value.length; fila++) {
    for (let columna = gridState.value[0].length - 2; columna >= 0; columna--) {
      if (gridState.value[fila][columna] !== 0) {
        while (
          columna < gridState.value[0].length - 1 &&
          gridState.value[fila][columna + 1] === 0
        ) {
          gridState.value[fila][columna + 1] = gridState.value[fila][columna];
          gridState.value[fila][columna] = 0;
          columna++;
          moved = true;
        }
        if (
          columna < gridState.value[0].length - 1 &&
          gridState.value[fila][columna + 1] === gridState.value[fila][columna]
        ) {
          const num = (gridState.value[fila][columna + 1] *= 2);
          gridState.value[fila][columna] = 0;
          moved = true;
          puntos.value += num;
        }
      }
    }
  }
  generarNumero(moved);
};

const handleKeyDown = (event) => {
  const key = event.key;
  if (key === "ArrowUp") {
    console.log("Arriba");
    moverArriba();
  } else if (key === "ArrowDown") {
    console.log("Abajo");
    moverAbajo();
  } else if (key === "ArrowLeft") {
    console.log("Izquierda");
    moverIzquierda();
  } else if (key === "ArrowRight") {
    console.log("Derecha");
    moverDerecha();
  }
};
const getStyle = (value) => {
  let style = {
    background: "#eee4da", // Estilo predeterminado
    fontSize: "50px", // Tamaño de fuente predeterminado
  };

  switch (value) {
    case 4:
      style.background = "#F5DDB2";
      break;
    case 8:
      style.background = "#f2b179";
      break;
    case 16:
      style.background = "#f59563";
      style.color = "#fff";
      break;
    case 32:
      style.background = "#f67c5f";
      style.color = "#fff";
      break;
    case 64:
      style.background = "#f65e3b";
      style.color = "#fff";
      break;
    case 128:
      style.background = "#edcf72";
      style.fontSize = "40px";
      break;
    case 256:
      style.background = "#edcc61";
      style.fontSize = "40px";
      break;
    case 512:
      style.background = "#edc850";
      style.fontSize = "40px";
      break;
    case 1024:
      style.background = "#edc53f";
      style.fontSize = "35px";
      break;
    case 2048:
      style.background = "#edc22e";
      style.fontSize = "35px";
      break;
    default:
      break;
  }

  return style;
};

const newGame = () => {
  for (let fila = 0; fila < gridState.value[0].length; fila++) {
    for (let columna = 0; columna < gridState.value.length; columna++) {
      gridState.value[fila][columna] = 0;
    }
  }
  puntos.value = 0;
  generarNumero(true);
  generarNumero(true);
};

onMounted(() => {
  generarNumero(true);
  generarNumero(true);
});
</script>
<style>

</style>
