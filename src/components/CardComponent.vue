<script setup>
import { ref } from "vue";

const isFlipped = ref(false);
const isDisabled = ref(false);
const emit = defineEmits(["onFlip"]);

const props = defineProps({
  imgBackFace: {
    type: String,
    required: true,
  },
  card: {
    type: [Number, String, Array, Object],
  },
  cardContext: {
    type: Array,
    default: () => [],
  },
});

const onToggeFlipCart = () => {
  if (isDisabled.value) return false;
  isFlipped.value = !isFlipped.value;
  if (isFlipped.value) emit("onFlip", props.card);
};

const onFlipBackCard = () => {
  isFlipped.value = false;
};
const isCorrect = () => {
  isDisabled.value = true;
};
defineExpose({
  onFlipBackCard,
  isCorrect,
});
</script>
<template>
  <div
    class="card"
    :class="isDisabled ? 'disabled' : ''"
    :style="{
      height: `${(920 - 16 * 4) / Math.sqrt(cardContext.length) - 16}px`,
      width: `${
        (((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4) * 2
      }px`,
    }"
  >
    <div
      class="card_inner"
      :class="{ isFlipped: isFlipped }"
      @click="onToggeFlipCart"
    >
      <div class="card_face card_face_front">
        <div
          class="card_content"
          :style="{
            'background-size': `${
              (((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
              4 /
              3
            }px ${
              (((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card_face card_face_back">
        <div
          class="card_content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + props.imgBackFace)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card_inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card.disabled .card_inner {
  cursor: default;
}

.card_inner.isFlipped {
  transform: rotateY(-180deg);
}

.card_face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 18px 3px rgba(0, 0, 0, 0.2);
}

.card_face_front .card_content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  height: 100%;
  width: 100%;
}

.card_face_back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card_face_back .card_content {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
  height: 100%;
  width: 100%;
}
</style>
