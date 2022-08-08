<script setup lang="ts">
import { computed, ref, toRefs } from "vue";
import jsonData from "../data.json";

const props = defineProps({
  tab: {
    type: Number,
    required: true,
  },
  planetColor: {
    type: String,
    required: true,
  },
});

const activeCategory = ref("overview");
const source = ref("");
const img = ref("");
const imgGeology = ref("");
const imgGeologyShow = ref(false);

const innerHtmlContent = computed(() => {
  switch (activeCategory.value) {
    case "internal":
      source.value = jsonData[props.tab].internal.source;
      img.value = jsonData[props.tab].images.internal;
      imgGeologyShow.value = false;
      return jsonData[props.tab].internal.content;
    case "geology":
      source.value = jsonData[props.tab].geology.source;
      img.value = jsonData[props.tab].images.internal;
      imgGeology.value = jsonData[props.tab].images.geology;
      imgGeologyShow.value = true;
      return jsonData[props.tab].geology.content;
    default:
      source.value = jsonData[props.tab].overview.source;
      img.value = jsonData[props.tab].images.overview;
      imgGeologyShow.value = false;
      return jsonData[props.tab].overview.content;
  }
});
</script>

<template>
  <div class="btn-group-mobile-container">
    <div class="btn-group-mobile">
      <div>
        <button
          class="btn-nav btn-category-nav-mobile"
          @click="activeCategory = 'overview'"
        >
          <p class="btn-group-mobile-text">OVERVIEW</p>
        </button>
      </div>
      <div>
        <button
          class="btn-nav btn-category-nav-mobile"
          @click="activeCategory = 'internal'"
        >
          <p class="btn-group-mobile-text">STRUCTURE</p>
        </button>
      </div>
      <div>
        <button
          class="btn-nav btn-category-nav-mobile"
          @click="activeCategory = 'geology'"
        >
          <p class="btn-group-mobile-text">SURFACE</p>
        </button>
      </div>
    </div>
    <div class="divider"></div>
  </div>
  <div class="main-grid">
    <div class="planet-img-wrapper">
      <transition name="rotate">
        <img :src="img" :key="img" alt="" class="planet-img" />
      </transition>
      <transition name="fade-in-delay">
        <div v-if="imgGeologyShow" class="imgGeology">
          <transition name="fade-in">
            <img :src="imgGeology" :key="imgGeology" alt="" />
          </transition>
        </div>
      </transition>
    </div>
    <div class="planet-info">
      <div>
        <h1>{{ jsonData[props.tab].name }}</h1>
        <p v-html="innerHtmlContent"></p>
        <p>
          <span class="opace"
            >Source :
            <a target="_blank" v-bind:href="source"
              >Wikipedia
              <img
                style="margin-bottom: -0.1rem; margin-left: 0.5rem"
                src="../assets/icon-source.svg"
                alt="" /></a
          ></span>
        </p>
      </div>
      <div class="btn-group">
        <div>
          <button
            class="btn-default btn-category"
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
            class="btn-default btn-category"
            @click="activeCategory = 'internal'"
            :class="{ active: activeCategory === 'internal' }"
          >
            <h3>
              <span class="opace" style="margin-right: 2rem">02</span>
              STRUCTURE
            </h3>
          </button>
        </div>
        <div>
          <button
            class="btn-default btn-category"
            @click="activeCategory = 'geology'"
            :class="{ active: activeCategory === 'geology' }"
          >
            <h3>
              <span class="opace" style="margin-right: 2rem">03</span>SURFACE
            </h3>
          </button>
        </div>
      </div>
    </div>
  </div>
  <div class="stats-grid">
    <div class="stat-item">
      <h4 class="opace">ROTATION TIME</h4>
      <h2 class="">{{ jsonData[props.tab].rotation }}</h2>
    </div>
    <div class="stat-item">
      <h4 class="opace">REVOLUTION TIME</h4>
      <h2>{{ jsonData[props.tab].revolution }}</h2>
    </div>
    <div class="stat-item">
      <h4 class="opace">RADIUS</h4>
      <h2>{{ jsonData[props.tab].radius }}</h2>
    </div>
    <div class="stat-item">
      <h4 class="opace">AVERAGE TEMP.</h4>
      <h2>{{ jsonData[props.tab].temperature }}</h2>
    </div>
  </div>
</template>

<style scoped lang="scss">
@import "../assets/settings.scss";

/* animations */
/*
.rotate-enter-from,
.rotate-leave-to {
  opacity: 0;
  transition: all 500ms;
}
*/
.rotate-enter-active {
  transition: transform 1s cubic-bezier(0.47, 1.64, 0.41, 0.8),
    opacity 1s ease-in-out;
  transition-delay: 0.5s;
}

.rotate-leave-active {
  transition: all 0.7s ease-in-out;
}

.rotate-enter-from {
  opacity: 0;
  transform: translate(-4.5rem, 4.5rem);
}

.rotate-leave-to {
  opacity: 0;
  transform: translate(4rem, -4rem);
}

.fade-in-leave-active,
.fade-in-enter-active {
  transition: all 0.5s ease-in-out;
}

.fade-in-enter-from {
  opacity: 0;
  transform: translateY(2rem);
}

.fade-in-leave-to {
  opacity: 0;
}

.fade-in-delay-leave-active {
  transition: all 0.5s ease-in-out;
}

.fade-in-delay-enter-active {
  transition: all 0.5s ease-in-out;
  transition-delay: 0.5s;
}

.fade-in-delay-enter-from {
  opacity: 0;
  transform: translateY(2rem);
}

.fade-in-delay-leave-to {
  opacity: 0;
}

/* layout styles */

.main-grid {
  display: grid;
  grid-template-columns: 60vw 40vw;
  margin-top: 8rem;
}

.planet-info {
  display: grid;
  grid-template-columns: 1fr;
  max-width: 22rem;
  margin-right: 6rem;
}

.planet-img-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  min-height: min(28rem, 75vw);
}
.planet-img {
  max-width: 90%;
  max-height: 100%;
  position: absolute;
}
.imgGeology {
  position: absolute;
  top: 60%;
  img {
    width: min(12rem, 40vw);
    position: absolute;
    transform: translateX(-50%);
  }
}

.btn-group-mobile-container {
  display: none;
}

.btn-group-mobile {
  display: flex;
  width: 100%;
  justify-content: space-between;
  max-width: min(30rem, calc(100vw - 4rem));
  margin: 0 auto;
}

.btn-category-nav-mobile {
  opacity: 0.5;
  padding: 1rem 0;
  transition: all 250ms ease-in-out;
}

.btn-category-nav-mobile:hover,
.btn-category-nav-mobile:focus {
  opacity: 1;
  box-shadow: inset 0 -0.25rem v-bind("planetColor");
}

.btn-group-mobile-text {
  font-size: 0.5625rem;
  font-weight: 700;
  line-height: 0.5625rem;
  letter-spacing: 0.120625rem;
  font-family: "League Spartan", sans-serif;
  margin: 0.1rem 0 0 0;
  padding: 0;
}

.stats-grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  margin: 6rem auto;
  gap: 1rem;
  max-width: min(calc(20vw + 44rem), calc(100vw - 12rem));
}

.stat-item {
  border: 1px rgba($color: white, $alpha: 0.2) solid;
  padding: 1rem;
  display: flex;
  flex-direction: column;
}

/* buttons */

.active {
  background-color: v-bind("planetColor") !important;
  border: 1px v-bind("planetColor") solid !important;
}

/* media queries */
@media only screen and (max-width: $md) {
  .main-grid {
    grid-template-columns: 1fr;
    margin-top: 2.5rem;
  }
  .planet-info {
    grid-template-columns: 1fr 1fr;
    max-width: 100%;
    margin: 4rem 1rem 0rem 1rem;
    align-items: center;
  }
  .btn-category {
    width: 80%;
    margin-left: 20%;
  }
  .stats-grid {
    max-width: 100%;
    margin: 2rem 1rem 4rem 1rem;
  }
}

@media only screen and (max-width: 45rem) {
  .stats-grid {
    grid-template-columns: 1fr;
  }
  .stat-item {
    flex-direction: row;
    justify-content: space-between;
  }
}

@media only screen and (max-width: $sm-large) {
  .btn-group {
    display: none;
  }
  .planet-info {
    grid-template-columns: 1fr;
    text-align: center;
  }
  .btn-group-mobile-container {
    display: block;
  }
}

@media only screen and (max-width: $sm) {
}
</style>