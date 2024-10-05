<script setup>
const emit = defineEmits(["startAgain"]);
defineProps({
  timer: {
    type: Number,
    required: true,
  },
});

const startAgain = () => {
  emit("startAgain");
};
</script>
<template>
  <div class="screen">
    <h1>🎉 Congratulations 🎉</h1>
    <p>{{ Math.round(timer / 1000) }} seconds</p>
    <button @click="startAgain">Start again</button>
  </div>
</template>
