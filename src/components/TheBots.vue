<template>
  <div class="bot-container" @click="1 + 1">
    <BotCard
      v-for="(bot, index) in props.bots"
      :key="index"
      :bot="bot"
      :total="props.bots.length"
    ></BotCard>
  </div>
</template>

<script setup>
import { defineProps } from "vue"

// Components
import BotCard from "@/components/BotCard.vue"

// We pass bots so that we can make the api call before this component is loaded while the header animation is playing
const props = defineProps(["bots"])
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
