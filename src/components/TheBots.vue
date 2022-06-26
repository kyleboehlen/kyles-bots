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

const props = defineProps(["bots"])
const flipTriggerIndex = ref(0)

const updateFlipTriggerIndex = (newIndex) => {
  console.log(newIndex)
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

.botcard-enter-active,
.botcard-leave-active {
  transition: all 10s ease;
}
.botcard-enter-from,
.botcard-leave-to {
  opacity: 0;
  transition-delay: calc(5s * (var(--total) - var(--i)));
}

@media only screen and (max-width: 1050px) {
  .bot-container {
    flex-wrap: wrap;
  }
}
</style>
