<template>
  <div class="w-full h-full">
    <div class="w-full h-full bg-black flex justify-center items-center flex-col" v-show="startup">
      <img :src="applelogo" alt="" width="50px">
      <div class="w-64 h-4 mt-8 bg-gray-700 rounded-full relative overflow-hidden border-2 border-gray-600 shadow-lg">
        <div class="absolute inset-0 bg-gradient-to-r from-gray-400 via-gray-300 to-gray-400 w-1/2 animate-slide"></div>
      </div>
    </div>
    <div class="w-full h-full flex flex-col" v-show="!startup">
      <MainBar title="Unlock Isabelle's IPod"/>
      <div class="w-full h-full grid grid-cols-2 bg-white flex-1" v-show="!startup">
        <div class="w-full h-full flex justify-center items-center flex-col">
          <p class="font-bold text-sm text-center">{{ question }}</p>
          <input
              type="text"
              v-model="answerInput"
              class="w-full px-4 py-1 my-4 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:outline-none"
              placeholder="Type your answer here..."
          />
          <button
              @click="() => {
                i = 0
                onConfirm()
              }"
              class="px-4 py-2 text-sm font-medium bg-blue-600 rounded-md"
              :class="{'text-gray-600 bg-gray-100': i == 1, 'text-white bg-blue-600': i == 0}"
          >
            Unlock
          </button>
        </div>

        <div class="w-full h-full flex justify-center items-center bg-white relative" @click="() => {
            i = 1
            onConfirm()
          }">
          <video :src="introvideo" class="h-[95%] absolute" :class="{'brightness-50': i == 1 && paused}" ref="video"/>
          <div v-if="i == 1 && paused" class="absolute left-0 top-0 w-full h-full flex justify-center items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                 stroke="currentColor" class="size-22 fill-white">
              <path stroke-linecap="round" stroke-linejoin="round"
                    d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.347a1.125 1.125 0 0 1 0 1.972l-11.54 6.347a1.125 1.125 0 0 1-1.667-.986V5.653Z"/>
            </svg>


          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import introvideo from "@/assets/intro/intro.mp4"
import applelogo from "@/assets/intro/apple_logo.svg"
import errorsfx from "@/assets/error.m4a"
import {onMounted, onUnmounted, ref} from "vue";
import {emitter} from "@/config.ts";
import MainBar from "@/components/MainBar.vue";

const question = "What’s the word, both bold and cheeky,\n" +
    "That fits this egg, so small and sneaky?\n" +
    "A playful trait, a little brassy—"
const answerInput = ref("")
const answer = "sassy"
const video = ref<HTMLVideoElement>()
const paused = ref<boolean>(true);
const startup = ref<boolean>(true);

const audio = new Audio();

setTimeout(() => {
  startup.value = false;
}, 3000)

const i = ref(1);

function onConfirm() {
  if (i.value == 0) {
    if (answerInput.value.toLowerCase() === answer) {
      localStorage.setItem("intro", "true");
      emitter.emit("changeMenu", "main");
    } else {
      audio.src = errorsfx;
      audio.play();
    }
  } else {
    pausePlayVideo();
  }
}

function onDown() {
  i.value = 1;
}

function onUp() {
  i.value = 0;
}

function pausePlayVideo() {
  video.value!.paused ? video.value!.play() : video.value!.pause()
  paused.value = video.value!.paused
}


onMounted(() => {
  emitter.on("down", onDown);
  emitter.on("up", onUp);
  emitter.on("confirm", onConfirm);
  emitter.on("pauseplay", pausePlayVideo);
})

onUnmounted(() => {
  emitter.off("down", onDown);
  emitter.off("up", onUp);
  emitter.off("confirm", onConfirm);
  emitter.off("pauseplay", pausePlayVideo);
})
</script>

<style scoped>
@keyframes slide {
  0% {
    transform: translateX(-100%);
  }
  100% {
    transform: translateX(200%);
  }
}

.animate-slide {
  animation: slide 2s infinite linear;
}
</style>