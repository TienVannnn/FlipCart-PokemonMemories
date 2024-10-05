<template>
  <div class="screen">
    <h1>Interact Component here</h1>
    <card-component
      v-for="(card, index) in cardContext"
      :key="index"
      :imgBackFace="`images/${card}.png`"
      :card="card"
    />
  </div>
</template>

<script setup>
import CardComponent from "./CardComponent.vue";
defineProps({
  cardContext: {
    type: Array,
    default: () => [],
  },
});
</script>
