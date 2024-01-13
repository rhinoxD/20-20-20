<script setup>
import { ref, onMounted } from 'vue'

const isRunning = ref(false)
const faceit = ref(null)
const airhorn = ref(null)
let flag = true
const m = ref(0)
const s = ref(0)

onMounted(() => start())

let countdownDate = new Date().setSeconds(new Date().getSeconds() + 1201);
let timerInterval;

function start() {
  airhorn.value.play().catch(err => {
    // catch err
  })
  startCountdown();
  timerInterval = setInterval(startCountdown, 1000);
}

function reset() {
  isRunning.value = false
  countdownDate = new Date().setSeconds(new Date().getSeconds() + 1201);
  m.value = 20
  s.value = "00"
}

const startCountdown = () => {
  isRunning.value = true
  const now = new Date().getTime();
  const countdown = new Date(countdownDate).getTime();

  const difference = (countdown - now) / 1000;

  if (difference < 1) {
    flag = !flag
    if (faceit.value != null && airhorn.value != null) {
      if (flag) {
        countdownDate = new Date().setSeconds(new Date().getSeconds() + 1201);
        airhorn.value.play().catch(err => {
          // catch err
        })
      } else {
        countdownDate = new Date().setSeconds(new Date().getSeconds() + 21);
        faceit.value.play().catch(err => {
          // catch err
        })
      }
    }
  }

  let minutes = Math.floor((difference % (60 * 60)) / 60).toString().length == 1 ? `0${Math.floor((difference % (60 * 60)) / 60)}` : Math.floor((difference % (60 * 60)) / 60);
  let seconds = Math.floor(difference % 60).toString().length == 1 ? `0${Math.floor(difference % 60)}` : Math.floor(difference % 60);

  m.value = minutes == -1 ? "00" : minutes
  s.value = seconds == -1 ? "00" : seconds
};
</script>

<template>
  <main class="flex flex-col items-center justify-center bg-gray-900 text-white p-8 h-screen">
    <p class="text-6xl md:text-8xl font-bold font-mono">{{ m }} : {{ s }}</p>

    <section class="flex  justify-center mt-5" id="timer" aria-live="polite">
      <audio class="hidden" ref='faceit' volume="0.3"
        src="https://www.myinstants.com/media/sounds/faceit-lone-wolf-howling_160k.mp3" />
      <!-- src="https://ergonomictrends.com/20-20-20-rest-eyes-health-tool/seagullsound.mp3" /> -->

      <audio class="hidden" ref='airhorn' volume="0.2"
        src="https://www.myinstants.com/media/sounds/airhorn-far-lethal-company.mp3" />
      <!-- src="https://ergonomictrends.com/20-20-20-rest-eyes-health-tool/clockchimesound.mp3" /> -->

      <button
        class=" px-4 py-2 rounded-lg border border-red-500 hover:bg-red-600 focus:outline-none transition disabled:cursor-not-allowed"
        type="button" @click="reset">
        Reset
      </button>
    </section>

    <p class="mt-6 text-center italic text-red-500">***Allow audio and video permissions in the browser to get audio
      alerts.***</p>
  </main>
</template>
