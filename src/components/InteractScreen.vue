<template>
  <div class="screen">
    <h1>Interact Component here</h1>
    <card-component
      v-for="(card, index) in cardContext"
      :key="index"
      :imgBackFace="`images/${card}.png`"
      :card="{ index, valueCard: card }"
      ref="cards"
      @onFlip="checkRule"
    />
  </div>
</template>

<script setup>
import { ref, nextTick } from "vue";
import CardComponent from "./CardComponent.vue";

const props = defineProps({
  cardContext: {
    type: Array,
    default: () => [],
  },
});

const emit = defineEmits(["onFinish"]);

const rules = ref([]);
const cards = ref([]);
const countCorrect = ref(0);

// Hàm kiểm tra rule
const checkRule = async (card) => {
  if (rules.value.length === 2) return;
  rules.value.push(card);

  if (
    rules.value.length === 2 &&
    rules.value[0].valueCard === rules.value[1].valueCard
  ) {
    console.log("Correct match");
    cards.value[rules.value[0].index].isCorrect();
    cards.value[rules.value[1].index].isCorrect();
    rules.value = [];
    countCorrect.value++;
    if (countCorrect.value == props.cardContext.length / 2) {
      console.log("done");
      setTimeout(() => {
        emit("onFinish");
      }, 900);
    }
  } else if (
    rules.value.length === 2 &&
    rules.value[0].valueCard !== rules.value[1].valueCard
  ) {
    console.log("Wrong match");
    await nextTick();
    setTimeout(() => {
      cards.value[rules.value[0].index].onFlipBackCard();
      cards.value[rules.value[1].index].onFlipBackCard();
      rules.value = [];
    }, 800);
  }
};
</script>
