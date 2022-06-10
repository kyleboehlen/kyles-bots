<template>
</template>

<script setup>
</script>

<style>
@font-face {
  font-family: "ShadowsIntoLight";
  src: local("ShadowsIntoLight-Regular"),
    url(@/assets/ShadowsIntoLight.ttf) format("truetype");
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
}
</style>
