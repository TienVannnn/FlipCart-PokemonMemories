<script setup>
const emit = defineEmits(["onStart"]);

const onStart = (totalOfBlocks) => {
  emit("onStart", { totalOfBlocks });
};
</script>

<template>
  <div class="screen">
    <h1>POKE MEMORIES</h1>
    <h3>Select mode to start game</h3>
    <div class="actions">
      <button @click="onStart(16)">
        <span>4*4</span>
        <span>Easy</span>
      </button>
      <button @click="onStart(36)">
        <span>6*6</span>
        <span>Normal</span>
      </button>
      <button @click="onStart(64)">
        <span>8*8</span>
        <span>Hard</span>
      </button>
      <button @click="onStart(100)">
        <span>10*10</span>
        <span>Super Hard</span>
      </button>
    </div>
  </div>
</template>
