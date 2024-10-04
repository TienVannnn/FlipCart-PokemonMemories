<template>
  <div>
    <main-screen v-if="statusMatch === 'default'" />
    <InteractScreen v-if="statusMatch == 'match'" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import InteractScreen from "./components/InteractScreen.vue";
import MainScreen from "./components/MainScreen.vue";

const statusMatch = ref("default");
</script>

<style></style>
