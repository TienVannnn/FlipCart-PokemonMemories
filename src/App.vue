<template>
  <div>
    <main-screen
      v-if="statusMatch === 'default'"
      @on-start="onHandleBeforeStart($event)"
    />
    <InteractScreen
      v-if="statusMatch == 'match'"
      :cardContext="settings.cardContext"
      @onFinish="onGetResult"
    />
    <ResultScreen
      :timer="timer"
      v-if="statusMatch == 'result'"
      @startAgain="startAgain"
    />
    <p class="copyright">
      This game owned by LVT -
      <a href="mailto:tienlevan78py@gmail.com">view here</a>
    </p>
  </div>
</template>

<script setup>
import { reactive, ref } from "vue";
import InteractScreen from "./components/InteractScreen.vue";
import MainScreen from "./components/MainScreen.vue";
import { shuffled } from "./utils/array";
import ResultScreen from "./components/ResultScreen.vue";

const statusMatch = ref("default");
const timer = ref(0);
const settings = reactive({
  totalOfBlocks: 0,
  cardContext: [],
  startedAt: null,
});

const onHandleBeforeStart = (config) => {
  settings.totalOfBlocks = config.totalOfBlocks;
  const firstCard = Array.from(
    { length: settings.totalOfBlocks / 2 },
    (_, i) => i + 1
  );
  const secondCard = [...firstCard];
  const cards = [...firstCard, ...secondCard];
  settings.cardContext = shuffled(shuffled(shuffled(shuffled(cards))));
  settings.startedAt = new Date().getTime();
  statusMatch.value = "match";
};

const onGetResult = () => {
  timer.value = new Date().getTime() - settings.startedAt;
  statusMatch.value = "result";
};

const startAgain = () => {
  statusMatch.value = "default";
};
</script>

<style>
.copyright {
  position: fixed;
  left: 50%;
  transform: translateX(-50%);
  bottom: 1.5rem;
  color: var(--light);
  z-index: 3;
  font-size: 1.5rem;
}

.copyright a {
  color: #f4dc26;
}
</style>
