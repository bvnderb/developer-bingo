<template>
  <div class="bingo-app">
    <h1>Developer Bingo</h1>
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
</template>

<script setup>
import { ref } from 'vue';

const DEV_MESSAGES = [
  "Het werkt op mijn machine",
  "Even snel iets aanpassen",
  "De klant wil nog een kleine aanpassing",
  "Nog snel de release pushen voor het weekend",
  "Wie heeft deze code geschreven? (jijzelf, 3 maanden geleden)",
  "Deze website trekt op niks!",
  "It's not a bug, it's a feature",
  "We doen het even zonder tests",
  "De deadline is morgen",
  "Waarom staat dit in productie?",
  "Het lag aan de cache",
  "Ik heb er nog nooit van gehoord maar hoeveel uur heb ik?",
  "Stack Overflow is down (moderne intake: Claude ligt weer plat)",
  "Gisteren werkte dit nog wel",
  "Console.log('hier')",
  "Dat zou eigenlijk niet mogelijk moeten zijn",
  "De documentatie is outdated",
  "Reboot fixes everything!",
  "Nu werkt het, ik weet niet waarom",
  "git blame toont mijn eigen naam",
  "De klant belt op vrijdagmiddag om 16u",
  "Even snel een fix deployen",
  "Nog één koffie en dan begin ik",
  "Copy paste van Stack Overflow",
  "AI kan het weer niet",
]

const shuffle = (arr) => [...arr].sort(() => Math.random() - 0.5);

const buildBoard = () => {
  const shuffled = shuffle(DEV_MESSAGES).slice(0, 24);
  shuffled.splice(12, 0, 'FREE');
  return shuffled.map((text, index) => ({
    id: index,
    text: text,
    marked: index === 12,
    isFree: index === 12
  }));
};

const cells = ref(buildBoard());

const markCell = (cell) => {
  if (cell.isFree) return;
  const target = cells.value.find(c => c.id === cell.id);
  target.marked = !target.marked;
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background-color: #1a1a2e;
  color: white;
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  padding: 2rem;
}

.bingo-app {
  text-align: center;
  max-width: 700px;
  width: 100%;
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
</style>
