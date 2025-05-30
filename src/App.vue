<script setup>
import Cursor from '@/components/Cursor.vue'
import Footer from '@/components/Footer.vue'
import Level from '@/components/Level.vue'
import Toolbox from '@/components/Toolbox.vue'
import Contact from '@/components/Contact.vue'
import Task from '@/components/Task.vue'
import Loading from '@/components/Loading.vue'
import Background from '@/components/Background.vue'
import { ref } from 'vue'

const loading = ref(true)
const percent = ref(1)
const l2dOnly = ref(true)

import NProgress from 'nprogress'

NProgress.start()

const load = setInterval(() => {
  percent.value = NProgress.status
  if (document.readyState === 'complete' && window.loadComplete === true) {
    NProgress.done()
    percent.value = 1
    setTimeout(() => {
      loading.value = false
    }, 2000)
    clearInterval(load)
  }
}, 1)

const switchL2D = () => {
  l2dOnly.value = !l2dOnly.value
}
</script>

<template>
  <transition name="loading">
    <Loading v-if="loading" :percent="percent"></Loading>
  </transition>
  <div id="background"></div>
  <main v-if="!loading">
    <Suspense>
      <Background :l2dOnly="l2dOnly" @update:changeL2D="l2dOnly = $event"></Background>
    </Suspense>
    <transition name="up">
      <Level v-if="!l2dOnly"></Level>
    </transition>
    <Toolbox :l2dOnly="l2dOnly" @switch="switchL2D"></Toolbox>
    <transition name="left">
      <Contact v-if="!l2dOnly"></Contact>
    </transition>
    <Task :l2dOnly="l2dOnly"></Task>
    <transition name="down">
      <Footer v-if="!l2dOnly"></Footer>
    </transition>
    <div id="curtain"></div>
  </main>
  <Cursor></Cursor>
</template>

<style scoped>
main {
  display: flex;
  flex-direction: column;
}

#app:has(.rotate-notification) main,
#app:has(.rotate-notification) #background,
#app:has(.rotate-notification) .loading_wrapper {
  display: none;
}

.loading-leave-to {
  opacity: 0;
}

.loading-leave-from {
  opacity: 1;
}

.loading-leave-active {
  transition: opacity 0.5s ease-in-out;
}

.up-leave-to,
.up-enter-from {
  transform: translateY(-300px);
}

.up-leave-from,
.up-enter-to {
  transform: translateY(0);
}

.down-leave-to,
.down-enter-from {
  transform: translateY(300px) skew(-20deg);
}

.down-leave-from,
.down-enter-to {
  transform: translateY(0) skew(-20deg);
}

@media screen and (max-width: 495px) {
  .down-leave-to,
  .down-enter-from {
    transform: translateY(300px);
  }

  .down-leave-from,
  .down-enter-to {
    transform: translateY(0);
  }

  .up-leave-to,
  .up-enter-from {
    transform: translateY(-300px) skew(-10deg);
  }

  .up-leave-from,
  .up-enter-to {
    transform: translateY(0) skew(-10deg);
  }
}

.left-leave-to,
.left-enter-from {
  transform: translateX(-300px);
}

.left-leave-from,
.left-enter-to {
  transform: translateX(0);
}

.up-leave-active,
.down-leave-active,
.left-leave-active {
  transition: transform 0.3s ease-in;
}

.up-enter-active,
.down-enter-active,
.left-enter-active {
  transition: transform 0.3s ease-out;
}

#background {
  background-image: url('/shitim/Event_Main_Stage_Bg.png');
  background-position: center;
  background-size: cover;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: -1;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}
</style>

<style>
main, .arco-overlay, .loading_wrapper {
  zoom: 0.4;
}

@media only screen and (min-width: 650px) and (min-height: 330px) {
  main, .arco-overlay, .loading_wrapper {
    zoom: 0.5;
  }
}

@media only screen and (min-width: 700px) and (min-height: 350px) {
  main, .arco-overlay, .loading_wrapper {
    zoom: 0.6;
  }
}

@media only screen and (min-width: 850px) and (min-height: 375px) {
  main, .arco-overlay, .loading_wrapper {
    zoom: 0.7;
  }
}

@media only screen and (min-width: 950px) and (min-height: 400px) {
  main, .arco-overlay, .loading_wrapper {
    zoom: 0.8;
  }
}

@media only screen and (min-width: 1060px) and (min-height: 500px) {
  main, .arco-overlay, .loading_wrapper {
    zoom: 0.9;
  }
}

@media only screen and (min-width: 1175px) and (min-height: 700px) {
  main, .arco-overlay, .loading_wrapper {
    zoom: 1;
  }
}

.arco-modal-body:has(.id-card) {
    max-width: unset !important;
  }
</style>