<template>
    <div class="container">
        <p class="score">
            {{ player1Name }} ({{ symbols[0] }}): {{ player1Score }}
        </p>
        <p class="score">
            {{ player2Name }} ({{ symbols[1] }}): {{ player2Score }}
        </p>
    </div>
    <div class="container">
        <button class="btn" @click="resetScores">Reiniciar Marcador</button>
    </div>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue';

const props = defineProps(['player1Name', 'player2Name', 'player1Score', 'player2Score', 'symbols']);
const emit = defineEmits();

const resetScores = () => {
    emit('reset');
};
</script>

<style>
.score {
    font-size: 24px;
    margin-bottom: 10px;

}

.container {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 25px;
}

.btn {
    margin: 15px 0 15px 0;
    padding: 10px 20px 10px 20px;
    border-radius: 25px;
    cursor: pointer;
    background-color: rgb(119, 159, 204);   
}

.btn:hover {
    background-color: rgb(196, 223, 253);
    color: black;
}
</style>