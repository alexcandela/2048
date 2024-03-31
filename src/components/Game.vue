<template>
    <main @keydown="handleKeyDown" tabindex="0" class="main">
        <div class="grid" id="grid">
            <div v-for="(fila, indexFila) in gridState" :key="indexFila" class="fila">
                <div v-for="(celda, indexCelda) in fila" :key="indexCelda" class="celda">
                    <span v-if="celda === 4" :style="{ background: '#FFB07E' }">{{ celda === 0 ? ' ' : celda }}</span>
                    <span v-else-if="celda === 8" :style="{ background: '#F69050' }">{{ celda === 0 ? ' ' : celda }}</span>
                    <span v-else-if="celda === 16" :style="{ background: '#B85B21' }">{{ celda === 0 ? ' ' : celda }}</span>
                    <span v-else-if="celda === 32" :style="{ background: '#F86D6D' }">{{ celda === 0 ? ' ' : celda }}</span>
                    <span v-else-if="celda === 64" :style="{ background: '#F84949' }">{{ celda === 0 ? ' ' : celda }}</span>
                    <span v-else-if="celda === 128" :style="{ background: '#F82D2D' }">{{ celda === 0 ? ' ' : celda }}</span>
                    <span v-else>{{ celda === 0 ? ' ' : celda }}</span>
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
    [0, 0, 0, 0],
]);

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
                    gridState.value[columna - 1][fila] *= 2;
                    gridState.value[columna][fila] = 0;
                    moved = true;
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
                    gridState.value[columna + 1][fila] *= 2;
                    gridState.value[columna][fila] = 0;
                    moved = true;
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
                    gridState.value[fila][columna - 1] *= 2;
                    gridState.value[fila][columna] = 0;
                    moved = true;
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
                    gridState.value[fila][columna + 1] *= 2;
                    gridState.value[fila][columna] = 0;
                    moved = true;
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

onMounted(() => {
    generarNumero(true);
    generarNumero(true);
});
</script>
<style lang=""></style>
