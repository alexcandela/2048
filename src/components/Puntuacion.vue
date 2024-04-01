<template>
    <div class="puntuacion">
        <div class="puntuacion-puntos"><span>Puntación: {{ puntos }}</span></div>
        <div class="puntuacion-newGame">
            <button @click="newGame()" class="btn btn-newGame">New Game</button>
        </div>
    </div>
</template>
<script setup>
import { defineProps, ref } from 'vue';

const { puntos, newGame } = defineProps(['puntos', 'newGame']);
</script>
<style>
    
</style>