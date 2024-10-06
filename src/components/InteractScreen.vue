<template>
  <div class="screen">
    <div
      class="screen_inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardContext.length)
        }px`,
      }"
    >
      <card-component
        v-for="(card, index) in cardContext"
        :key="index"
        :imgBackFace="`images/${card}.png`"
        :card="{ index, valueCard: card }"
        ref="cards"
        :cardContext="cardContext"
        @onFlip="checkRule"
      />
    </div>
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

<style scoped>
.screen {
  width: 100%;
  height: auto;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen_inner {
  width: calc(424px);
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
