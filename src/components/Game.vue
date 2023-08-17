<template>
    <div class="game-container">
        <div v-if="!gameStarted" class="input-container">
            <PlayerInput @start="startGame" />
        </div>
        <div v-else class="gameplay-container">
            <Scoreboard :player1Name="players[0]" :player2Name="players[1]" :player1Score="player1Score"
                :player2Score="player2Score" :symbols="symbols" @reset="resetGame" />
            <Board v-if="gameStarted" :board="board" :makeMove="makeMove" />
            <div v-if="gameOver" class="button-container">
                <button @click="restartGame" class="restart-button">Start Again</button>
                <button v-if="showSwitchButton" @click="switchSymbols" class="switch-button">Change Symbols</button>
            </div>
            <div v-if="gameOutcome" class="game-outcome-message">{{ gameOutcome }}</div>
        </div>
    </div>
</template>
<script setup>
import { ref } from 'vue';
import PlayerInput from './PlayerInput.vue';
import Scoreboard from './ScoreBoard.vue';
import Board from './Board.vue';

// Estado del juego
const player1Name = ref('');
const player2Name = ref('');
const player1Score = ref(0);
const player2Score = ref(0);
const currentPlayerIndex = ref(0);
const gameStarted = ref(false);
const gameOver = ref(false);
const players = ref([]);
const symbols = ref(['X', 'O']);
const showSwitchButton = ref(false);
const board = ref(Array(3).fill(Array(3).fill('')));
const gameOutcome = ref("");
// Función para cambiar los símbolos
const switchSymbols = () => {
    const tempSymbol = symbols.value[0];
    symbols.value[0] = symbols.value[1];
    symbols.value[1] = tempSymbol;
};

// Función para comenzar el juego
const startGame = (p1Name, p2Name) => {
    players.value = [p1Name, p2Name];
    gameStarted.value = true;
    currentPlayerIndex.value = 0;
    board.value = Array(3).fill().map(() => Array(3).fill(''));
    gameOver.value = false;
};

// Función para realizar un movimiento
const makeMove = (row, col) => {
    // Verificar si la celda está vacía y el juego no ha terminado
    if (!board.value[row][col] && !gameOver.value) {
        const newBoard = board.value.map(row => [...row]);
        newBoard[row][col] = symbols.value[currentPlayerIndex.value];
        board.value = newBoard;

        if (checkWin(symbols.value[currentPlayerIndex.value])) {
            // Incrementar el puntaje del jugador y mostrar el botón de cambio de símbolos
            showSwitchButton.value = true;
            gameOver.value = true;
            const winningPlayer = currentPlayerIndex.value === 0 ? players.value[0] : players.value[1];
            const winningSymbol = symbols.value[currentPlayerIndex.value];
            gameOutcome.value = `${winningPlayer} you won with the symbol ${winningSymbol}!`;
        } else if (checkDraw()) {
            // Mostrar el botón de cambio de símbolos si hay empate
            showSwitchButton.value = true;
            gameOver.value = true;
            gameOutcome.value = "Its a draw! Try again!";
            gameOver.value = true;
        } else {
            // Cambiar al siguiente jugador
            currentPlayerIndex.value = 1 - currentPlayerIndex.value;
            showSwitchButton.value = false;
            gameOutcome.value = "";
        }
    }
};

// Función para verificar si alguien ha ganado
const checkWin = (player) => {
    // Combinaciones ganadoras posibles
    const winCombos = [
        [[0, 0], [0, 1], [0, 2]], [[1, 0], [1, 1], [1, 2]], [[2, 0], [2, 1], [2, 2]], // Filas
        [[0, 0], [1, 0], [2, 0]], [[0, 1], [1, 1], [2, 1]], [[0, 2], [1, 2], [2, 2]], // Columnas
        [[0, 0], [1, 1], [2, 2]], [[0, 2], [1, 1], [2, 0]] // Diagonales
    ];

    // Verificar si alguna combinación ganadora se ha completado
    return winCombos.some(combination =>
        combination.every(([row, col]) => board.value[row][col] === player)
    );
};

// Función para verificar si hay empate
const checkDraw = () => board.value.flat().every(cell => cell !== '');

// Función para reiniciar el juego
const restartGame = () => {
    gameOver.value = false;
    gameOutcome.value = ""; // Reiniciar el mensaje
    gameOver.value = false;
    board.value = Array(3).fill().map(() => Array(3).fill(''));
    currentPlayerIndex.value = 1 - currentPlayerIndex.value;
    showSwitchButton.value = false;
};

// Función para reiniciar y volver a la pantalla de inicio
const resetGame = () => {
    gameStarted.value = false;
    player1Name.value = '';
    player2Name.value = '';
    player1Score.value = 0;
    player2Score.value = 0;
    switchSymbols();
    currentPlayerIndex.value = 1 - currentPlayerIndex.value;
    showSwitchButton.value = false;
};
</script>

<style scoped>
.game-container {
    display: flex;
    justify-content: center;
    align-items: center;
}

.input-container {
    text-align: center;
    padding: 2rem;
}

.gameplay-container {
    text-align: center;
}

.button-container {
    margin-top: 1rem;
}

.restart-button,
.switch-button {
    margin: 0 0.5rem;
    padding: 10px;
    border-radius: 20px;
    background-color: rgb(139, 190, 226);
}

.game-outcome-message {
    margin-top: 1rem;
    font-size: 1.2rem;
    font-weight: bold;
    color: #ff5722;
}
</style>