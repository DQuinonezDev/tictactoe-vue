<template>
    <div class="game-board">
        <div v-for="(row, rowIndex) in board" :key="rowIndex" class="row">
            <div v-for="(cell, colIndex) in row" :key="colIndex" class="cell"
                :class="{ 'player-x': cell === 'X', 'player-o': cell === 'O' }" @click="makeMove(rowIndex, colIndex)">
                {{ cell }}
            </div>
        </div>
    </div>
</template>
  
<script setup>
import { defineProps } from 'vue';

const game = defineProps({
    board: Array,
    currentPlayer: String,
    makeMove: Function,
    lastMove: Object
});
</script>
<style scoped>
.game-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.game-header {
    font-size: 24px;
    margin-bottom: 20px;
}

.game-board {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.row {
    display: flex;
}

.cell {
    width: 80px;
    height: 80px;
    border: 2px solid #ccc;
    margin: 0.3px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    border-radius: 15px;
    cursor: pointer;
    box-shadow: 2px 2px 2px 2px gray;
    transition: background-color 0.3s ease;
}

.cell.player-x {
    color: #d46e62;
    font-size: 55px;
}

.cell.player-o {
    color: #1972ae;
    font-size: 55px;

}

.cell.last-move {
    animation: highlight 1s;
}

@keyframes highlight {
    0%, 100% {
        background-color: transparent;
    }
    50% {
        background-color: #ffeea8;
    }
}
</style>