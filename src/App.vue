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

const apiUrl = import.meta.env.VITE_API_URL
const animationEnd = ref(false)
const bots = ref([])

const showBots = computed(() => {
  return animationEnd.value && bots.value.length > 0
})

provide("show", showBots)

onMounted(() => {
  console.log(apiUrl + "/bots")
  axios.get(apiUrl + "/bots").then((response) => {
    const body = response.data
    if (body.success) {
      const botObj = body.bots
      console.log(botObj)

      botObj.forEach((bot) => {
        bots.value.push({
          name: bot.name,
          desc: bot.desc,
          img: bot.img,
          url: bot.invite_url,
        })
      })
    }
    console.log(bots.value)
  })
})
</script>

<style>
@font-face {
  font-family: "ShadowsIntoLight";
  src: local("ShadowsIntoLight-Regular"),
    url(@/assets/ShadowsIntoLight.ttf) format("truetype");
}

@font-face {
  font-family: "Electrolize";
  src: local("Electrolize"), url(@/assets/Electrolize.ttf) format("truetype");
}

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
