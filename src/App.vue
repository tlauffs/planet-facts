<script setup lang="ts">
import jsonData from "./data.json";
const tab = 1;
const planetColor = jsonData[tab].color;
const info = jsonData;
</script>

<template>
  <div>
    <img :src="'src/' + jsonData[tab].images.planet" alt="">
    <h1>{{ jsonData[tab].name }}</h1>
    <p>{{ jsonData[tab].overview.content }}</p>
  </div>
</template>

<style scoped lang="scss">
.planet-color {
  color: v-bind("planetColor");
}
</style>
