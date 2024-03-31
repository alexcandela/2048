<template>
    <main @keydown="handleKeyDown" tabindex="0" class="main">
        <div class="grid" id="grid">
            <div v-for="(fila, indexFila) in gridState" :key="indexFila" class="fila">
                <div v-for="(celda, indexCelda) in fila" :key="indexCelda" class="celda">
                    <span>{{ celda == 0 ? ' ' : celda }}</span>
                </div>
            </div>
        </div>
    </main>
</template>
<script setup>
import { onMounted, ref } from "vue";

const gridState = ref([
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0],
    [0, 0, 0, 0]
]);

const handleKeyDown = (event) => {
  const key = event.key;
  if (key === 'ArrowUp') {
    console.log('Arriba');
    generarNumero()
  } else if (key === 'ArrowDown') {
    console.log('Abajo');
    generarNumero()
  } else if (key === 'ArrowLeft') {
    console.log('Izquierda');
    generarNumero()
  } else if (key === 'ArrowRight') {
    console.log('Derecha');
    generarNumero()
  }
};

const detectarCeldasVacias = () => {
    const celdasVacias = [];
    for (let i = 0; i < gridState.value.length; i++) {
        for (let j = 0; j < gridState.value[i].length; j++) {
            if (gridState.value[i][j] === 0) {
                celdasVacias.push({fila: i, columna: j});
            }            
        }        
    }
    return celdasVacias;
}

const generarNumero = () => {
    const celdasVacias = detectarCeldasVacias();
    const randomNumber = Math.random() < 0.8 ? 2 : 4;
    const celdaVacia = Math.floor(Math.random() * celdasVacias.length);
    const {fila, columna} = celdasVacias[celdaVacia];
    gridState.value[fila][columna] = randomNumber;
};

onMounted(() => {
    generarNumero();
    generarNumero();
})

</script>
<style lang="">

</style>