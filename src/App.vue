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
  console.log("total of block: ", settings.totalOfBlocks);
  const firstCard = Array.from(
    { length: settings.totalOfBlocks / 2 },
    (_, i) => i + 1
  );
  const secondCard = [...firstCard];
  const cards = [...firstCard, ...secondCard];
  settings.cardContext = shuffled(shuffled(shuffled(shuffled(cards))));
  console.log(settings.cardContext);
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

<style></style>
