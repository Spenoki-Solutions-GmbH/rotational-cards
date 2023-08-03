<script setup lang="ts">
import { ref } from 'vue'
import CoverImage from '@/assets/cover01.jpg'

// show cover image
const firstLoad = ref(true)
// random pokemon of the first generation (No 1 to 151)
const randomPokemon = ref([...Array(10)].map(() => Math.round(Math.random() * 151)))
// four rows with five cards each
const randomCards = ref([...Array(20)])
// uncovered cards
const flippedCards = ref([...Array(20)].map(() => false))
// currently shown card
const flippedTemp = ref(-1)
const flippedTemp2 = ref(-1)

// fill cards array with random pairs of pokemon
for (let i = 0, j = 0; i < 20; i += 2, j++) {
  randomCards.value[i] = randomPokemon.value[j]
  randomCards.value[i + 1] = randomPokemon.value[j]
}
// randomize cards
randomCards.value.sort(() => Math.random() - 0.5)

function flip(i: number): void {
  if (flippedTemp.value === -1) {
    flippedTemp.value = i
  } else if (flippedTemp2.value === -1) {
    flippedTemp2.value = i

    setTimeout(() => {
      flippedTemp.value = -1
      flippedTemp2.value = -1
    }, 500)
  }

  const pokemon1 = randomCards.value[flippedTemp.value]
  const pokemon2 = randomCards.value[flippedTemp2.value]

  if (pokemon1 === pokemon2) {
    flippedCards.value[flippedTemp.value] = true
    flippedCards.value[flippedTemp2.value] = true
  }
}

function restart() {
  randomPokemon.value = [...Array(10)].map(() => Math.round(Math.random() * 151))

  for (let i = 0, j = 0; i < 20; i += 2, j++) {
    randomCards.value[i] = randomPokemon.value[j]
    randomCards.value[i + 1] = randomPokemon.value[j]
  }
  randomCards.value.sort(() => Math.random() - 0.5)
}
</script>

<template>
  <div v-if="firstLoad"
    id="cover"
    @click="firstLoad = false"
  >
    <button>click anywhere to start</button>
  </div>

  <ol v-else>
    <li v-for="card,i in randomCards"
      @click="flip(i)"
      :class="{ flipped: flippedCards[i] || flippedTemp === i || flippedTemp2 === i }"
      :style="{ backgroundImage: `url(https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${card}.png)` }"
    >
      &nbsp;
    </li>
  </ol>
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
ol {
  list-style: none;
  display: flex;
  flex-flow: row wrap;
  width: 900px;
  height: 720px;
  padding: 0;
  border: 1px solid white;
  background-color: #222;
}
li {
  width: 176px;
  height: 176px;
  background: #111;
  border: 2px solid #222;
  background-color: black;
  background-position: center;
  background-repeat: no-repeat;
  background-size: 0;
  transform: rotateX(180deg);
  background-color: #111;
  transition: transform .3s ease;
}
li.flipped {
  transform: rotateX(0deg);
  transform-origin: center;
  background-size: contain;
}
</style>
