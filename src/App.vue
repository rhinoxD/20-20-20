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

// let test = 11
let countdownDate = new Date().setSeconds(new Date().getSeconds() + 1201);
let timerInterval;

function start() {
  startCountdown();
  timerInterval = setInterval(startCountdown, 1000);
}

function reset() {
  isRunning.value = false
  // test = 1201
  countdownDate = new Date().setSeconds(new Date().getSeconds() + 1201);
  // minutesElem.innerHTML = formatTime(20, "minute");
  // secondsElem.innerHTML = formatTime(0, "second");
  m.value = 20
  s.value = "00"
  // clearInterval(timerInterval);
}

const startCountdown = () => {
  // test -= 1
  isRunning.value = true
  const now = new Date().getTime();
  const countdown = new Date(countdownDate).getTime();

  const difference = (countdown - now) / 1000;

  if (difference < 1) {
    flag = !flag
    if (seagull.value != null && bell.value != null) {
      if (flag) {
        countdownDate = new Date().setSeconds(new Date().getSeconds() + 1201);
        bell.value.play().catch(err => {
          // catch err
        })
      } else {
        countdownDate = new Date().setSeconds(new Date().getSeconds() + 21);
        seagull.value.play().catch(err => {
          // catch err
        })
      }
    }
  }

  let minutes = Math.floor((difference % (60 * 60)) / 60).toString().length == 1 ? `0${Math.floor((difference % (60 * 60)) / 60)}` : Math.floor((difference % (60 * 60)) / 60);
  let seconds = Math.floor(difference % 60).toString().length == 1 ? `0${Math.floor(difference % 60)}` : Math.floor(difference % 60);

  m.value = minutes == -1 ? "00" : minutes
  s.value = seconds == -1 ? "00" : seconds

  // minutesElem.innerHTML = formatTime(minutes, "minute");
  // secondsElem.innerHTML = formatTime(seconds, "second");
};

// const endCountdown = () => {
//   isRunning.value = false;
//   clearInterval(timerInterval);
//   console.log("end", test)
//   console.log("end", countdownDate)
// };

</script>

<template>
  <main class="flex flex-col items-center justify-center bg-gray-900 text-white p-8 h-screen">
    <p class="text-7xl md:text-8xl font-bold font-mono">{{ m }} : {{ s }}</p>

    <section class="flex  justify-center  mt-5" id="timer" aria-live="polite">
      <audio class="hidden" ref='seagull'
        src="https://ergonomictrends.com/20-20-20-rest-eyes-health-tool/seagullsound.mp3" />

      <!-- <audio class="hidden" ref='bell' src="./public/audio/clockchimesound.mp3" /> -->
      <audio class="hidden" ref='bell'
        src="https://ergonomictrends.com/20-20-20-rest-eyes-health-tool/clockchimesound.mp3" />
      <!-- <button v-if="isRunning" -->
      <!--   class="px-4 py-2 rounded-lg border border-green-500 hover:bg-green-600 focus:outline-none transition" -->
      <!--   type="button" @click="endCountdown"> -->
      <!--   Stop -->
      <!-- </button> -->
      <!-- <button v-else-if="!isRunning" -->
      <!--   class="px-4 py-2 rounded-lg border border-green-500 hover:bg-green-600 focus:outline-none transition" -->
      <!--   type="button" @click="start"> -->
      <!--   Start -->
      <!-- </button> -->

      <button
        class=" px-4 py-2 rounded-lg border border-red-500 hover:bg-red-600 focus:outline-none transition disabled:cursor-not-allowed"
        type="button" @click="reset">
        Reset
      </button>
    </section>

    <p class="mt-6 text-center italic text-red-500">***Allow audio and video permissions in the browser to get audio alerts.***</p>
  </main>
</template>
