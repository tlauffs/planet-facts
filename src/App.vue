<script setup lang="ts">
import { ref, watch } from "vue";
import jsonData from "./data.json";
import MainComponent from "./components/Main.vue";

const tab = ref(2);
const openNav = ref(false);
const planetColor = ref(jsonData[tab.value].color);
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
          <span>
            <h4>{{ planet }}</h4>
          </span>
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
    <MainComponent :tab="tab" :planetColor="planetColor" />
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

/* media queries */

@media only screen and (max-width: $md) {
  .header {
    flex-direction: column;
    align-items: center;
  }
  .btn-nav-desktop {
    padding: 1rem 0.75rem;
  }
  .btn-nav:focus,
  .btn-nav:hover {
    opacity: 1;
    box-shadow: inset 0 -0.25rem var(--color-hover);
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
