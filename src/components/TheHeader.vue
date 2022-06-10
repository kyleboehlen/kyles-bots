<template>
  <header>
    <div
      id="logo"
      :class="{
        loader: animationEndCount == 0,
        move: animationEndCount >= 1,
        fixed: animationEndCount >= 2,
      }"
      @animationend="animationEnd"
    >
      <img src="@/assets/logo.png" />
      <Transition name="write" @after-enter="animationEnd">
        <h1 v-if="animationEndCount >= 2">Kyle's Bots</h1>
      </Transition>
    </div>

    <Transition name="fade">
      <a
        v-if="animationEndCount >= 3"
        href="https://www.kyleboehlen.com"
        target="_blank"
      >
        <h2>My Portfolio</h2>
        <ExternalLink></ExternalLink>
      </a>
    </Transition>
  </header>
</template>

<script setup>
import { ref, defineEmits } from "vue"

import ExternalLink from "@/components/ExternalLink.vue"

const animationEndCount = ref(0)
const emit = defineEmits(["animation-end"])

const animationEnd = () => {
  animationEndCount.value++
  if (animationEndCount.value >= 2) {
    emit("animation-end")
  }
}
</script>

<style scoped>
/* LOGO/TITLE DIV */
#logo.loader {
  animation: grow 3s ease-in-out;
  height: 35vh;
  left: 50%;
  position: absolute;
  top: 50%;
  transform: translate(-50%, -50%);
}

#logo.move {
  animation: move 1.5s ease-in;
  align-items: center;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  height: 12.5vh;
  transform: none;
}

#logo.fixed {
  padding-left: 2.5vw;
  padding-top: 1.5vh;
}

/* LOGO */
img {
  max-height: 100%;
}

/* ALL HEADER TEXT */
h1,
h2 {
  color: #dbdce1;
  font-family: ShadowsIntoLight;
  margin: 0;
}

/* TITLE HEADER */
h1 {
  font-family: ShadowsIntoLight;
  font-size: 5rem;
  overflow: hidden;
  white-space: nowrap;
  margin-left: 1vw;
  margin-bottom: 2vh;
}

/* PORTFOLIO LINK */
a {
  align-items: center;
  display: flex;
  font-size: 1.25rem;
  margin-right: 1.25vw;
  margin-top: 0.5vh;
  position: absolute;
  right: 0;
  text-decoration: none;
  top: 0;
}

a:hover {
  text-decoration-color: #dbdce1;
  text-shadow: -1.2px 0.5px 0px #dbdce1;
}

/* PORTFOLIO EXTERNAL LINK ICON */
svg {
  display: inline-block;
  fill: #dbdce1;
  height: 1.25rem;
  margin-left: 0.75vw;
  width: 1.25rem;
}

a:hover > svg {
  filter: drop-shadow(-1.2px 0.5px 0px #dbdce1);
}

/* VUE TRANSITION CLASSES */
.fade-enter-active {
  transition: opacity 1s ease-out;
}

.fade-enter-from {
  opacity: 0;
}

.write-enter-active {
  transition: width 1.5s ease;
}

.write-enter-from {
  width: 0px;
}

.write-enter-to {
  width: 100%;
}

/* MOBILE STYLES */
@media only screen and (max-width: 1000px) {
  h1 {
    font-size: 4rem;
  }
}

@media only screen and (max-width: 650px) {
  h1 {
    font-size: 3rem;
    margin: 0;
    text-align: center;
  }

  a {
    font-size: 0.75rem;
    margin-top: 0.25vh;
  }

  svg {
    height: 1rem;
    margin-left: 1vw;
    margin-right: 1.5vw;
    width: 1rem;
  }

  #logo.loader {
    animation: grow-mobile 3s ease-in-out;
    width: 100%;
    display: flex;
    justify-content: center;
    padding-top: 2.5vh;
    height: 50vh;
    left: 0%;
    position: absolute;
    top: 25vh;
    transform: none;
  }

  #logo.move {
    animation: move-mobile 1.5s ease-in;
    align-items: center;
    flex-direction: column;
    height: 25vh;
    transform: none;
    width: 100%;
  }

  #logo.fixed {
    padding-left: 0;
    padding-top: 1vh;
  }

  img {
    max-height: 45%;
  }

  .write-enter-active {
    transition: width 1.5s ease;
  }

  .write-enter-from {
    width: 0px;
  }

  .write-enter-to {
    width: 100%;
  }
}

/* CUSTOM ANIMATIONS */
@keyframes grow {
  from {
    height: 0vh;
    opacity: 20%;
  }
  to {
    height: 35vh;
    opacity: 100%;
  }
}

@keyframes grow-mobile {
  from {
    height: 0vh;
    opacity: 20%;
  }
  to {
    height: 50vh;
    opacity: 100%;
  }
}

@keyframes move {
  0% {
    left: 50%;
    height: 35vh;
    position: absolute;
    top: 50%;
    transform: translate(-50%, -50%);
  }
  99% {
    padding-left: 2.5vw;
    position: absolute;
    padding-top: 1.5vh;
    left: 0;
    top: 0;
  }
  100% {
    padding-top: 1.5vh;
    padding-left: 2.5vw;
    position: relative;
    transform: none;
  }
}

@keyframes move-mobile {
  0% {
    width: 100%;
    padding-top: 2.5vh;
    height: 50vh;
    position: absolute;
    top: 25vh;
  }
  100% {
    padding-top: 1vh;
    position: relative;
    height: 25vh;
    top: 0;
  }
}
</style>
