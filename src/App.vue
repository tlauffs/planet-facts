<script setup lang="ts">
import { ref, onMounted, watch } from "vue";
import jsonData from "./data.json";

const tab = ref(0);
const openNav = ref(false);
const planetColor = ref(jsonData[tab.value].color);
const activeCategory = ref("overview");
const planetList = [
  "mercury",
  "venus",
  "earth",
  "mars",
  "jupiter",
  "saturn",
  "uranus",
  "neptune",
];

watch(tab, (currentValue, oldValue) => {
  planetColor.value = jsonData[currentValue].color;
});

function styles(i: number) {
  return {
    "--color-hover": jsonData[i].color,
  };
}

function imgpath(){
  switch(activeCategory.value) {
    case 'internal':
      return jsonData[tab.value].images.internal;
    case 'geology':
      return jsonData[tab.value].images.geology;
    default:
      return jsonData[tab.value].images.overview;
  }
}
</script>

<template>
  <header class="header">
    <h5>THE PLANETS</h5>
    <div class="nav-flex">
      <div v-for="(planet, i) in planetList" :key="planet">
        <button
          class="btn-nav btn-nav-desktop"
          @click="tab = i"
          :style="styles(i)"
        >
          <h4>{{ planet }}</h4>
        </button>
      </div>
    </div>
    <div class="ham-menu">
      <button class="btn-nav" @click="openNav = true">
        <img src="./assets/icon-hamburger.svg" alt="" />
      </button>
    </div>
  </header>
  <div class="divider"></div>
  <div>
    <div class="main-grid">
      <div class="planet-img-wrapper">
        <img :src="imgpath()" alt="" class="planet-img" />
      </div>
      <div class="planet-info">
        <h1>{{ jsonData[tab].name }}</h1>
        <!--<p>{{ jsonData[tab].overview.content }}</p>-->
        <p>{{ jsonData[tab].overview.content }}</p>
        <p>
          <span class="opace"
            >Source :
            <a target="_blank" v-bind:href="jsonData[tab].overview.source"
              >Wikipedia
              <img
                style="margin-bottom: -0.1rem; margin-left: 0.5rem"
                src="./assets/icon-source.svg"
                alt="" /></a
          ></span>
        </p>
        <div>
          <button
            class="btn-default"
            @click="activeCategory = 'overview'"
            :class="{ active: activeCategory === 'overview' }"
          >
            <h3>
              <span class="opace" style="margin-right: 2rem">01</span>OVERVIEW
            </h3>
          </button>
        </div>
        <div>
          <button
            class="btn-default"
            @click="activeCategory = 'internal'"
            :class="{ active: activeCategory === 'internal' }"
          >
            <h3>
              <span class="opace" style="margin-right: 2rem">02</span>INTERNAL
              STRUCTURE
            </h3>
          </button>
        </div>
        <div>
          <button
            class="btn-default"
            @click="activeCategory = 'geology'"
            :class="{ active: activeCategory === 'geology' }"
          >
            <h3>
              <span class="opace" style="margin-right: 2rem">03</span>SURFACE
              GEOLOGY
            </h3>
          </button>
        </div>
      </div>
    </div>
  </div>
  <transition name="slide">
    <div class="sidenav" v-if="openNav">
      <div class="header">
        <h5>THE PLANETS</h5>
      </div>
      <div class="divider"></div>
      <div class="mobile-menu">
        <div v-for="(planet, i) in planetList" :key="planet">
          <button
            class="btn-nav btn-mobile-menu"
            @click="
              tab = i;
              openNav = false;
            "
          >
            <div
              class="circle"
              :style="{ 'background-color': jsonData[i].color }"
            ></div>
            <h6>{{ planet }}</h6>
            <img src="./assets/icon-chevron.svg" alt="" class="chevron" />
          </button>
          <div class="divider"></div>
        </div>
      </div>
    </div>
  </transition>
</template>

<style scoped lang="scss">
@import "./assets/settings.scss";
.planet-color {
  color: v-bind("planetColor");
}

/* navbar styles */

.header {
  margin: 0 2rem;
  display: flex;
  justify-content: space-between;
}
.nav-flex {
  display: flex;
  align-items: center;
}
.btn-nav-desktop {
  padding: 1.75rem 1rem;
  opacity: 0.75;
}
.btn-nav:focus,
.btn-nav:hover {
  opacity: 1;
  box-shadow: inset 0 0.25rem var(--color-hover);
}
/* mobile navbar styles */

.sidenav {
  position: fixed;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
  background: $base-color;
  overflow: auto;
  transition: all 250ms ease-in-out;
  display: none;
}

.slide-enter-from,
.slide-leave-to {
  transform: translateX(100vw);
}

.ham-menu {
  display: none;
}

.mobile-menu {
  margin: 2rem 1rem;
}

.btn-mobile-menu {
  display: flex;
  align-items: center;
  width: 100%;
  margin: 1.5rem 0;
}

.circle {
  width: 1rem;
  height: 1rem;
  border-radius: 1rem;
  margin-bottom: 0.2rem;
  margin-right: 1rem;
}

.chevron {
  margin-left: auto;
}

/* layout styles */

.main-grid {
  display: grid;
  grid-template-columns: 60vw 40vw;
  margin-top: 8rem;
}

.planet-info {
  max-width: 22rem;
  margin-right: 8rem;
}

.planet-img-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}
.planet-img {
  max-width: 90%;
}

/* buttons */

.active {
  background-color: v-bind("planetColor") !important;
  border: 1px v-bind("planetColor") solid !important;
}

/* media queries */

@media only screen and (max-width: $md) {
  .header {
    flex-direction: column;
    align-items: center;
  }
  .btn-nav-padding {
    padding: 1rem 0.5rem;
  }
  .main-grid {
    grid-template-columns: 1fr;
  }
}

@media only screen and (max-width: $sm) {
  .nav-flex {
    display: none;
  }
  .header {
    flex-direction: row;
  }
  .ham-menu {
    display: block;
  }
  .sidenav {
    display: block;
  }
}
</style>
