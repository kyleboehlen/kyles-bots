<template>
  <TheHeader @animation-end="animationEnd = true"></TheHeader>
  <TheBots :bots="bots"></TheBots>
</template>

<script setup>
import { onMounted, computed, ref, provide } from "vue"
import axios from "axios"

// Components
import TheHeader from "@/components/TheHeader.vue"
import TheBots from "@/components/TheBots.vue"

// KBPortfolio API endpoint
const apiUrl = import.meta.env.VITE_API_URL

// Passing the emit event on the end of the header logo animation to the bot cards
const animationEnd = ref(false)

// For holding the bots returned from the API
const bots = ref([])

// Only show the bots if we've successfully gotten them back from the api and the logo animation has ended
const showBots = computed(() => {
  return animationEnd.value && bots.value.length > 0
})

// Pass showing all the way to the cards, not TheBots, for animation delay sake
provide("show", showBots)

onMounted(() => {
  // Get the bots from the API
  axios.get(apiUrl + "/bots").then((response) => {
    const body = response.data

    // Veify we get a success true field back
    if (body.success) {
      const botObj = body.bots

      // Reformat to just the stuff we need
      botObj.forEach((bot) => {
        bots.value.push({
          name: bot.name,
          desc: bot.desc,
          img: bot.img,
          url: bot.invite_url,
        })
      })
    }
  })
})
</script>

<style>
/* Handwriting style font in the header */
@font-face {
  font-family: "ShadowsIntoLight";
  src: local("ShadowsIntoLight-Regular"),
    url(@/assets/ShadowsIntoLight.ttf) format("truetype");
}

/* Bot title/button font */
@font-face {
  font-family: "Electrolize";
  src: local("Electrolize"), url(@/assets/Electrolize.ttf) format("truetype");
}

/* Typewriter style font for bot desc */
@font-face {
  font-family: "SpecialElite";
  src: local("SpecialElite"), url(@/assets/SpecialElite.ttf) format("truetype");
}

html {
  height: 100vh;
  scroll-behavior: smooth;

  @media screen and (min-width: 320px) and (max-width: 767px) and (orientation: landscape) {
    width: 100vh;
    left: 0;
  }
}

body {
  background-color: #04092b;
  height: 100vh;
  margin: 0;
}

#app {
  height: 100%;
  display: flex;
  flex-direction: column;
}
</style>
