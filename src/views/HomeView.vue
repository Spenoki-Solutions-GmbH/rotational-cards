<script setup lang="ts">
import { ref } from 'vue'
import CoverImage from '@/assets/cover01.jpg'

const firstLoad = ref(true)
</script>

<template>
  <div v-if="firstLoad"
    id="cover"
    @click="firstLoad = false"
  >
    click anywhere to start
  </div>

  <div v-else>
    ... game not yet implemented ...
  </div>
</template>

<style scoped>
#cover {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: black url('@/assets/cover01.jpg') center no-repeat;
  background-size: cover;
  line-height: 144vh;
  text-align: center;
  font-size: 1.5em;
  color: black;
  text-shadow: 0 0 5px white;
  cursor: pointer;
}
</style>
