<script setup>
import { ref, computed, onMounted } from 'vue'

const setInterval1 = ref(null)
const elapsedTime = ref(null)
const MILLISECONDS = 10
const isRunning = ref(false)
const seagull = ref(null)
const bell = ref(null)
let flag = true

// const startTime = Date.now()

// on each tick

// let timePassedInMs = Date.now() - startTime
// let something = 0

// setInterval(() => {
//   timePassedInMs = Date.now() - startTime
//   something = elapsedTime.value - timePassedInMs
//   console.log("this is something", something)
//   console.log("this is timePassedInMs", timePassedInMs)
//   console.log("this is elapsedTime", elapsedTime.value)
//   console.log(Math.floor(something % 1000))
//   console.log(Math.floor((something / 1000) % 60))
//   console.log(Math.floor((something / (1000 * 60)) % 60))
//   console.log(Math.floor((something / (1000 * 60 * 60)) % 24))
//   // console.log(timePassedInMs / 1000)
//   // console.log(timePassedInMs / 1000 / 60)
//   // console.log(timePassedInMs / 1000 / 60 / 60)
//   // console.log(timePassedInMs / 1000 / 60 / 60 / 24)
// }, 1000)

function toggleTimer() {
  if (isRunning.value) {
    clearInterval(setInterval1.value)
  } else {
    setInterval1.value = setInterval(() => {
      if (elapsedTime.value <= 0) {
        if (seagull.value != null && bell.value != null) {
          // flag ? bell.value.play() : seagull.value.play()

          if (flag) {
            bell.value.play().catch(err => {
              // catch err
            })
          } else {
            seagull.value.play().catch(err => {
              // catch err
            })
          }
        }

        elapsedTime.value = flag ? 1200000 : 20000
        flag = !flag
      }

      elapsedTime.value -= MILLISECONDS
    }, MILLISECONDS)
  }
  isRunning.value = !isRunning.value
}

onMounted(() => toggleTimer())


function resetTimer() {
  clearInterval(setInterval1.value)
  isRunning.value = false
  elapsedTime.value = 1200000
}

const formattedTime = computed(() => {
  const milliseconds = elapsedTime.value % 1_000
  const seconds = Math.floor((elapsedTime.value / 1_000) % 60)
  const minutes = Math.floor((elapsedTime.value / (1_000 * 60)) % 60)
  const hours = Math.floor((elapsedTime.value / (1_000 * 60 * 60)) % 24)

  return `${hours.toString().padStart(2, '0')}:${minutes
    .toString()
    .padStart(2, '0')}:${seconds.toString().padStart(2, '0')}.${milliseconds
      .toString()
      .slice(0, 2)
      .padStart(2, '0')}`
})

const formattedTime2 = computed(() => {
  const milliseconds = elapsedTime.value % 1_000
  const seconds = Math.floor((elapsedTime.value / 1_000) % 60)
  const minutes = Math.floor((elapsedTime.value / (1_000 * 60)) % 60)
  const hours = Math.floor((elapsedTime.value / (1_000 * 60 * 60)) % 24)

  const tms = elapsedTime.value - timePassedInMs / 1000
  const ts = Math.floor(timePassedInMs / (1000 * 60))
  const tm = Math.floor(timePassedInMs / (1000 * 60 * 60))
  const th = Math.floor(timePassedInMs / (1000 * 60 * 60 * 24))

  return `${th.toString().padStart(2, '0')}:${tm
    .toString()
    .padStart(2, '0')}:${ts.toString().padStart(2, '0')}.${tms
      .toString()
      .slice(0, 2)
      .padStart(2, '0')}`
})

</script>
<template>
  <main class="flex flex-col items-center justify-center bg-gray-900 text-white p-8 h-screen">
    <p class="text-8xl font-bold font-mono">{{ formattedTime }}</p>
    <!-- <p class="text-8xl font-bold font-mono">{{ formattedTime2 }}</p> -->

    <section class="flex justify-center space-x-4">
      <!-- <audio class="hidden" ref='seagull' src="/audio/seagullsound.mp3" /> -->
      <audio class="hidden" ref='seagull'
        src="https://ergonomictrends.com/20-20-20-rest-eyes-health-tool/seagullsound.mp3" />

      <!-- <audio class="hidden" ref='bell' src="./public/audio/clockchimesound.mp3" /> -->
      <audio class="hidden" ref='bell'
        src="https://ergonomictrends.com/20-20-20-rest-eyes-health-tool/clockchimesound.mp3" />

      <button class="px-4 py-2 rounded-lg border border-green-500 hover:bg-green-600 focus:outline-none transition"
        type="button" @click="toggleTimer">
        {{ isRunning ? 'Stop' : 'Start' }}
      </button>

      <button
        class="px-4 py-2 rounded-lg border border-red-500 hover:bg-red-600 focus:outline-none transition disabled:cursor-not-allowed"
        type="button" @click="resetTimer" :disabled="isRunning">
        Reset
      </button>
    </section>
  </main>
</template>
