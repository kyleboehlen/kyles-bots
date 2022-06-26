<template>
  <Transition name="fade">
    <div
      v-if="show"
      class="flip-card"
      :style="{
        '--index': index,
        '--total': total,
        '--desktop-height': height + 'vw',
      }"
    >
      <div
        class="flip-card-inner"
        @click="activateFlipTrigger"
        :class="{ 'mobile-flip-trigger': showFlipTrigger }"
      >
        <div class="flip-card-front">
          <img :src="assetUrl + props.bot.img" alt="Bot Logo" />
        </div>
        <div class="flip-card-back">
          <p class="name">{{ props.bot.name }}</p>
          <p class="desc">{{ props.bot.desc }}</p>
          <button @click="addBot">Add Bot</button>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import {
  defineProps,
  inject,
  computed,
  ref,
  getCurrentInstance,
  defineEmits,
} from "vue"

// Get the bot details, the total number of bots, and the index of which bot card is flipped on mobile
const props = defineProps(["bot", "total", "flipTriggerIndex"])

// Asset url for s3 bucket
const assetUrl = import.meta.env.VITE_ASSET_URL

// For staggering the fade in animation until the rest has shown
const show = inject("show")

// Calculating width of card depending on how many bots there are (for desktop)
const height = computed(() => {
  const maxHeight = 80 / props.total
  if (maxHeight > 27.5) {
    return 27.5
  }
  return maxHeight
})

// Refs for handling mobile card flipping
const flipTriggerTimeout = ref(true)
const flipTriggerTimeoutFlag = ref(false)

// Need the index to hide this card if another card is flipped
const index = getCurrentInstance().vnode.key

// Define the event that we send to the parent when we trigger flipping this card
const emit = defineEmits("updateFlipTriggerIndex")

// Function for when we click on a card
const activateFlipTrigger = () => {
  // Timeout flag flipped to show
  flipTriggerTimeoutFlag.value = false

  // Send this index to the parent as flipped to close the rest of the cards
  emit("updateFlipTriggerIndex", index)

  // Set the timeout so that we don't keep it open for more than 3 seconds
  // Using this value makes sure that it's always three seconds from the most recent click, not from an old click
  flipTriggerTimeout.value = Date.now()

  // eslint-disable-next-line no-unused-vars
  var flipTriggerTimer = setTimeout(() => {
    // We don't just want to set a timeout tho, we just want to re compute showFlipTrigger after 3 seconds
    // If we just use the timeout we risk closing it too soon as someone flips back and forth between cards
    // and the timeout is still active and closes it on them
    flipTriggerTimeoutFlag.value = flipTriggerTimeoutFlag.value = true
  }, 3000)
}

const showFlipTrigger = computed(() => {
  return (
    props.flipTriggerIndex == index && // Make sure this card is the last one clicked
    Date.now() - flipTriggerTimeout.value < 3 && // That it hasn't been open for more than 3 seconds
    !flipTriggerTimeoutFlag.value // And this is for reactivity because computed won't update with Date.now() alone
  )
})

// Redirect to bot invite link
const addBot = () => {
  window.open(props.bot.url, "_blank")
}
</script>

<style scoped>
.flip-card {
  background-color: #04092b;
  display: inline-block;
  perspective: 1000px;
  height: var(--desktop-height);
  width: var(--desktop-height);
}

/* This container is needed to position the front and back side */
.flip-card-inner {
  background-color: #04092b;
  height: 100%;
  position: relative;
  text-align: center;
  transition: transform 0.8s;
  transform-style: preserve-3d;
  width: 100%;
}

img {
  border-radius: 5%;
  height: auto;
  object-fit: contain;
  width: 100%;
}

/* Do an horizontal flip when you move the mouse over the flip box container */
.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

/* FLIP TRIGGER FOR MOBILE */

/* Position the front and back side */
.flip-card-front,
.flip-card-back {
  -webkit-backface-visibility: hidden; /* Safari */
  backface-visibility: hidden;
  background-color: #04092b;
  border-radius: 5%;
  height: 100%;
  position: absolute;
  width: 100%;
}

/* Style the front side (fallback if image is missing) */
.flip-card-front {
  background-color: #04092b;
  border-radius: 5%;
  color: black;
}

/* Style the back side */
.flip-card-back {
  align-items: center;
  background-color: #04092b;
  border-radius: 5%;
  border: 5px solid white;
  box-sizing: border-box;
  color: white;
  display: flex;
  flex-direction: column;
  height: 100%;
  justify-content: space-between;
  padding: 3vh;
  transform: rotateY(180deg);
  width: 100%;
}

/* Text */
p.name,
button {
  color: #00ffff;
  font-family: "Electrolize";
  margin: 0;
}

p.name {
  /* Calculating font size based on how many bots there are */
  font-size: calc(4rem - calc(0.25rem * var(--total)));
}

button {
  /* Calculating font size based on how many bots there are */
  font-size: calc(3.5rem - calc(0.25rem * var(--total)));
}

p.desc {
  color: white;
  font-family: "SpecialElite";
  /* Calculating font size based on how many bots there are */
  font-size: calc(2.25rem - calc(0.15rem * var(--total)));
  margin: 0;
}

button {
  background-color: #04092b;
  border-radius: 10px;
  border: 2px solid white;
  font-size: 2rem;
  padding: 5%;
  width: 80%;
}

button:hover {
  box-shadow: -5px -5px 5px 0px rgba(255, 255, 255, 0.763);
  cursor: pointer;
  transform: translate(2.5px, 2.5px);
}

button:active {
  box-shadow: none;
  transform: none;
}

/* Transition in */
.fade-enter-active {
  transition: all 1.5s ease;
  /* Delay based on which index the bot card is so they fade in one after another */
  transition-delay: calc(1s + calc(0.5s * var(--index)));
}
.fade-enter-from {
  opacity: 0;
}

/* MOBILE STYLES */
@media only screen and (max-width: 1800px) {
  .flip-card-back {
    padding: 2vh;
  }
  p.name {
    font-size: calc(3.25rem - calc(0.25rem * var(--total)));
  }

  button {
    font-size: calc(2.5rem - calc(0.25rem * var(--total)));
  }

  p.desc {
    font-size: calc(2rem - calc(0.15rem * var(--total)));
  }
}

@media only screen and (max-width: 1250px) {
  .mobile-flip-trigger {
    transform: rotateY(180deg);
  }
}

@media only screen and (max-width: 1250px) {
  .flip-card-back {
    padding: 1.25vh;
  }
  p.name {
    font-size: calc(3rem - calc(0.25rem * var(--total)));
  }

  button {
    font-size: calc(2.25rem - calc(0.25rem * var(--total)));
  }

  p.desc {
    font-size: calc(1.75rem - calc(0.15rem * var(--total)));
  }
}

@media only screen and (max-width: 1050px) {
  .flip-card {
    height: 32.5vw;
    width: 32.5vw;
    margin-left: 5vw;
    margin-right: 5vw;
    margin-top: 2.5vw;
    margin-bottom: 2.5vw;
  }
}

@media only screen and (max-width: 850px) {
  .flip-card {
    height: 40vw;
    width: 40vw;
    margin-left: 2.5vw;
    margin-right: 2.5vw;
    margin-top: 2vw;
    margin-bottom: 0;
  }
}

@media only screen and (max-width: 850px) {
  .flip-card {
    height: 45vw;
    width: 45vw;
    margin-left: 0.5vw;
    margin-right: 0.5vw;
    margin-top: 2vw;
    margin-bottom: 0;
  }
}

@media only screen and (max-width: 650px) {
  .flip-card {
    height: 60vw;
    width: 60vw;
    margin: 0;
    margin-bottom: 5vw;
  }
}

@media only screen and (max-width: 500px) {
  .flip-card {
    height: 80vw;
    width: 80vw;
  }
}

@media only screen and (max-width: 375px) {
  .flip-card {
    height: 90vw;
    width: 90vw;
    margin-top: 2.5vw;
    margin-bottom: 2.5vw;
  }
}
</style>
