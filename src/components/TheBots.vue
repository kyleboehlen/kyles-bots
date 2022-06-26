<template>
  <div class="bot-container">
    <BotCard
      v-for="(bot, index) in props.bots"
      :key="index"
      :bot="bot"
      :total="props.bots.length"
      :flipTriggerIndex="flipTriggerIndex"
      @updateFlipTriggerIndex="(newIndex) => updateFlipTriggerIndex(newIndex)"
    ></BotCard>
  </div>
</template>

<script setup>
import { defineProps, ref } from "vue"

// Components
import BotCard from "@/components/BotCard.vue"

// We pass bots so that we can make the api call before this component is loaded while the header animation is playing
const props = defineProps(["bots"])

// Reactive index for which card is flipped on mobile
const flipTriggerIndex = ref(0)

// Update the index of the flipped card when the card emits it, passes to the other cards
const updateFlipTriggerIndex = (newIndex) => {
  flipTriggerIndex.value = newIndex
}
</script>

<style scoped>
.bot-container {
  align-items: center;
  display: flex;
  flex-direction: row;
  flex-grow: 1;
  flex-wrap: nowrap;
  justify-content: space-evenly;
  padding-bottom: 10vh;
  width: 100%;
}

@media only screen and (max-width: 1050px) {
  .bot-container {
    flex-wrap: wrap;
  }
}
</style>
