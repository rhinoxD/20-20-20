<script setup>
import { ref, onMounted } from 'vue'

const isRunning = ref(false)
const seagull = ref(null)
const bell = ref(null)
let flag = true
// let minutesElem = document.getElementById("minutes"),
//   secondsElem = document.getElementById("seconds");
const m = ref(0)
const s = ref(0)

onMounted(() => {
  // minutesElem = document.getElementById("minutes"),
  //   secondsElem = document.getElementById("seconds");

  start()
  // startCountdown();
  // timerInterval = setInterval(startCountdown, 1000);
})

let countdownDate = new Date().setSeconds(new Date().getSeconds() + 1200);

let timerInterval;

function start() {
  startCountdown();
  timerInterval = setInterval(startCountdown, 1000);
}

function reset() {
  isRunning.value = false
  countdownDate = new Date().setSeconds(new Date().getSeconds() + 1201);
  // minutesElem.innerHTML = formatTime(20, "minute");
  // secondsElem.innerHTML = formatTime(0, "second");
  m.value = 20
  s.value = "00"
  clearInterval(timerInterval);
}

const startCountdown = () => {
  isRunning.value = true
  const now = new Date().getTime();
  const countdown = new Date(countdownDate).getTime();

  const difference = (countdown - now) / 1000;

  if (difference < 1) {
    flag = !flag
    if (seagull.value != null && bell.value != null) {
      if (flag) {
        countdownDate = new Date().setSeconds(new Date().getSeconds() + 1200);
        bell.value.play().catch(err => {
          // catch err
        })
      } else {
        countdownDate = new Date().setSeconds(new Date().getSeconds() + 20);
        seagull.value.play().catch(err => {
          // catch err
        })
      }
    }
  }

  let minutes = Math.floor((difference % (60 * 60)) / 60).toString().length == 1 ? `0${Math.floor((difference % (60 * 60)) / 60)}` : Math.floor((difference % (60 * 60)) / 60);
  let seconds = Math.floor(difference % 60).toString().length == 1 ? `0${Math.floor(difference % 60)}` : Math.floor(difference % 60);

  m.value = minutes
  s.value = seconds

  // minutesElem.innerHTML = formatTime(minutes, "minute");
  // secondsElem.innerHTML = formatTime(seconds, "second");
};

const endCountdown = () => {
  isRunning.value = false;
  clearInterval(timerInterval);
};

</script>

<template>
  <main class="flex flex-col items-center justify-center bg-gray-900 text-white p-8 h-screen">
    <p class="text-8xl font-bold font-mono">{{ m }} : {{ s }}</p>

    <section class="flex  justify-center space-x-5 mt-5" id="timer" aria-live="polite">
      <button v-if="isRunning"
        class="px-4 py-2 rounded-lg border border-green-500 hover:bg-green-600 focus:outline-none transition"
        type="button" @click="endCountdown">
        Stop
      </button>
      <button v-else-if="!isRunning"
        class="px-4 py-2 rounded-lg border border-green-500 hover:bg-green-600 focus:outline-none transition"
        type="button" @click="start">
        Start
      </button>

      <button
        class="px-4 py-2 rounded-lg border border-red-500 hover:bg-red-600 focus:outline-none transition disabled:cursor-not-allowed"
        type="button" @click="reset" :disabled="isRunning">
        Reset
      </button>
    </section>
  </main>
</template>
