<template>
    <div class="game-setup">
        <h1 class="title">READY TO PLAY?</h1>
        <div class="player-inputs">
            <input class="player-input" v-model="player1Name" placeholder="Name Player 1">
            <input class="player-input" v-model="player2Name" placeholder="Name Player 2">
        </div>
        <button class="start-button" @click="startGame">Start Game</button>
    </div>
</template>
  
<script setup>
import { ref, defineEmits } from 'vue';

const player1Name = ref('');
const player2Name = ref('');

const emit = defineEmits();

const startGame = () => {
    emit('start', player1Name.value, player2Name.value);
};
</script>
  
<style scoped>
.game-setup {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 55vh;
    border-radius: 8px;
    box-shadow: 0px 2px 6px rgba(0, 0, 0, 0.1);
    padding: 30px;
}

.title {
    font-size: 28px;
    margin-bottom: 20px;
}

.player-inputs {
    display: flex;
    flex-direction: column;
    gap: 15px;
}

.player-input {
    padding: 12px;
    border: 1px solid #ccc;
    border-radius: 6px;
    width: 300px;
    font-size: 16px;
}

.player-input::placeholder {
    color: rgb(241, 230, 230);
    text-align: center;
}

.start-button {
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 6px;
    padding: 12px 24px;
    margin-top: 15px;
    font-size: 18px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.start-button:hover {
    background-color: #0056b3;
}
</style>
  