<template>
  <div class="w-full h-full bg-white flex flex-col">
    <MainBar title="Trivia Master"/>

    <div v-if="currQuestionI < triviaQuestions.length" class="flex-1">
      <p class="text-lg font-bold px-1">{{ triviaQuestions[currQuestionI].question }}</p>
      <hr class="border-t border-gray-300 mb-1"/>
      <MenuItem v-for="(item, i) in triviaQuestions[currQuestionI].options" :key="item" :text="item"
                :selected="i == currI"/>
    </div>

    <div v-else class="w-full h-full grid grid-cols-2 flex-1">
      <div class="flex justify-center items-center flex-col">
        <p class="text-2xl font-bold">{{victoryMessages[score]}}</p>
        <p class="font-bold">Score: {{ score }}</p>
      </div>

      <div class="flex justify-center items-center">
        <img :src="victoryImages[score]" alt="bigbom" class="w-[150px]">
      </div>
    </div>

  </div>
</template>

<script setup lang="ts">
import {onMounted, onUnmounted, ref} from "vue";
import {emitter} from "@/config.ts";
import MainBar from "@/components/MainBar.vue";
import MenuItem from "@/components/MenuItem.vue";

import song1 from "@/assets/games/1.mp4";
import song2 from "@/assets/games/2.mp4";
import song3 from "@/assets/games/3.mp4";
import song4 from "@/assets/games/4.mp4";
import song5 from "@/assets/games/5.mp4";

import correctsound from "@/assets/games/correct.ogg";
import wrongsound from "@/assets/games/wrong.mp4";

import victoryimg1 from "@/assets/games/1.jpeg";
import victoryimg2 from "@/assets/games/2.jpeg";
import victoryimg3 from "@/assets/games/3.jpeg";
import victoryimg4 from "@/assets/games/4.jpeg";
import victoryimg5 from "@/assets/games/5.jpeg";

const audio = new Audio();
const sfx = new Audio();
const currQuestionI = ref(0)
const currI = ref(0)
const score = ref(0);
const finalScoreAudio = [song1, song1, song2, song3, song4, song5]
const victoryImages = [victoryimg1, victoryimg1, victoryimg2, victoryimg3, victoryimg4, victoryimg5]
const victoryMessages = ["That's embarrassing…", "Really?", "I mean...", "Could be worse", "Almost there", "You achieved true Isabelle level"]

const triviaQuestions = [{
  question: "For what band was the lead singer the third option?",
  options: ["Green day", "Nirvana", "Hailstorm", "Led zeppelin"],
  answer: "Led zeppelin",
}, {
  question: "Eric Clapton wrote Layla for the wife of which beatle?",
  options: ["John lennon", "George Harrison", "Paul McCartney", "Ringo Star", "Damian Vlaicu"],
  answer: "George Harrison",

}, {
  question: "What famous singer has a life-size bronze statue in London's camden town district?",
  options: ["Robert fleet", "Amy winehouse", "Elton john", "George micheal"],
  answer: "Amy winehouse",
}, {
  question: "What band wrote I am the walrus that got banned from BBC?",
  options: ["Green Day", "Good charlotte", "The Cure", "Beatles"],
  answer: "Beatles",
}, {
  question: "Which band shortly had a female singer?",
  options: ["The Beatles", "Motley Crew", "Metallica", "Nirvana"],
  answer: "Motley Crew",
}]

function onDown() {
  const newI = currI.value + 1;
  if (newI <= triviaQuestions[0].options.length) currI.value = newI;
}

function onUp() {
  const newI = currI.value - 1;
  if (newI >= 0) currI.value = newI;
}

function onConfirm() {
  if (triviaQuestions[currQuestionI.value].options[currI.value] == triviaQuestions[currQuestionI.value].answer) {
    score.value += 1;
    sfx.src = correctsound;
  }
  else {
    sfx.src = wrongsound;
  }
  sfx.play();

  currQuestionI.value++;
  if (currQuestionI.value >= triviaQuestions.length) {
    audio.src = finalScoreAudio[score.value];
    audio.play();
  }
}


onMounted(() => {
  emitter.on("down", onDown);
  emitter.on("up", onUp);
  emitter.on("confirm", onConfirm);
})

onUnmounted(() => {
  emitter.off("down", onDown);
  emitter.off("up", onUp);
  emitter.off("confirm", onConfirm);
})
</script>


<style scoped>

</style>