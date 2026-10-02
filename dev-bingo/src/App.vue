<template>
  <div class="bingo-app">
    <h1>Developer Bingo</h1>
    <div v-if="hasBingo" class="bingo-message">🎉 BINGO! 🎉</div>
    <button @click="resetGame" class="reset-button">Nieuw spel</button>
    <div class="bingo-grid">
      <div
        v-for="cell in cells"
        :key="cell.id"
        class="bingo-cell"
        :class="{ marked: cell.marked, free: cell.isFree }"
        @click="markCell(cell)"
      >
        {{ cell.text }}
      </div>
    </div>
  </div>
  <footer>
    <p>Made by Brent Vander Bruggen.</p>
  </footer>
</template>

<script setup>
import { ref } from 'vue';

const DEV_MESSAGES = [
  'Het werkt op mijn machine',
  'Even snel iets aanpassen',
  'De klant wil nog een kleine aanpassing',
  'Nog snel de release pushen voor het weekend',
  'Wie heeft deze code geschreven? (jijzelf, 3 maanden geleden)',
  'Deze website trekt op niks!',
  "It's not a bug, it's a feature",
  'We doen het even zonder tests',
  'De deadline is morgen',
  'Waarom staat dit in productie?',
  'Het lag aan de cache',
  'Ik heb er nog nooit van gehoord maar hoeveel uur heb ik?',
  'Stack Overflow is down (moderne intake: Claude ligt weer plat)',
  'Gisteren werkte dit nog wel',
  "Console.log('hier')",
  'Dat zou eigenlijk niet mogelijk moeten zijn',
  'De documentatie is outdated',
  'Reboot fixes everything!',
  'Nu werkt het, ik weet niet waarom',
  'git blame toont mijn eigen naam',
  'De klant belt op vrijdagmiddag om 16u',
  'Even snel een fix deployen',
  'Nog één koffie en dan begin ik',
  'Copy paste van Stack Overflow',
  'AI kan het weer niet',
];

const shuffle = (arr) => [...arr].sort(() => Math.random() - 0.5);

const buildBoard = () => {
  const shuffled = shuffle(DEV_MESSAGES).slice(0, 24);
  shuffled.splice(12, 0, 'FREE');
  return shuffled.map((text, index) => ({
    id: index,
    text: text,
    marked: index === 12,
    isFree: index === 12,
  }));
};

const cells = ref(buildBoard());

const markCell = (cell) => {
  console.log('clicked', cell);
  if (cell.isFree) return;
  const target = cells.value.find((c) => c.id === cell.id);
  target.marked = !target.marked;
  hasBingo.value = checkBingo();
};

const checkBingo = () => {
  const board = cells.value;
  const lines = [
    [0, 1, 2, 3, 4],
    [5, 6, 7, 8, 9],
    [10, 11, 12, 13, 14],
    [15, 16, 17, 18, 19],
    [20, 21, 22, 23, 24], // rows
    [0, 5, 10, 15, 20],
    [1, 6, 11, 16, 21],
    [2, 7, 12, 17, 22],
    [3, 8, 13, 18, 23],
    [4, 9, 14, 19, 24], // columns
    [0, 6, 12, 18, 24],
    [4, 8, 12, 16, 20], // diagonals
  ];
  return lines.some((line) => line.every((i) => board[i].marked));
};

const hasBingo = ref(false);

const resetGame = () => {
  cells.value = buildBoard();
  hasBingo.value = false;
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #1a1a2e;
  color: white;
  font-family: Arial, sans-serif;
  padding: 2rem;
}

.bingo-app {
  flex: 1;
  text-align: center;
  max-width: 700px;
  width: 100%;
}

footer {
  margin-top: auto;
  padding: 1rem;
  color: #666;
  font-size: 0.8rem;
}

h1 {
  font-size: 2.5rem;
  margin-bottom: 1.5rem;
  color: #e94560;
}

.bingo-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 8px;
}

.bingo-cell {
  background-color: #16213e;
  border: 2px solid #0f3460;
  border-radius: 8px;
  padding: 12px 8px;
  font-size: 0.75rem;
  cursor: pointer;
  min-height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  transition: all 0.2s ease;
}

.bingo-cell:hover {
  border-color: #e94560;
  background-color: #0f3460;
}

.bingo-cell.marked {
  background-color: #e94560;
  border-color: #e94560;
  color: white;
}

.bingo-cell.free {
  background-color: #e94560;
  border-color: #e94560;
  font-weight: bold;
  font-size: 1rem;
}

.bingo-message {
  font-size: 3rem;
  color: #e94560;
  margin-bottom: 1rem;
  animation: pulse 0.5s infinite alternate;
}

@keyframes pulse {
  from {
    transform: scale(1);
  }
  to {
    transform: scale(1.1);
  }
}
.reset-button {
  background-color: #0f3460;
  color: white;
  border: 2px solid #e94560;
  border-radius: 8px;
  padding: 10px 24px;
  font-size: 1rem;
  cursor: pointer;
  margin-bottom: 1.5rem;
  transition: all 0.2s ease;
}

.reset-button:hover {
  background-color: #e94560;
}
</style>
