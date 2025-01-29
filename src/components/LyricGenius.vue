<template>
  <div class="w-full h-full bg-white flex flex-col">
    <MainBar title="Trivia Master"/>

    <div v-if="currQuestionI == -1" class="w-full h-full flex-1"
         :style="{ backgroundImage: `url(${titlecover})`, backgroundSize: 'cover', backgroundPosition: 'center'}">
      <p class="text-3xl text-white absolute bottom-18">Press confirm to continue</p>
    </div>
    <div v-else-if="currQuestionI < lyricsGeniusQuestions.length" class="flex-1">
      <p class="text-lg font-bold px-1">Song {{ currQuestionI + 1 }}</p>
      <hr class="border-t border-gray-300 mb-1"/>
      <MenuItem v-for="(item, i) in lyricsGeniusQuestions[currQuestionI].options" :key="item" :text="item"
                :selected="i == currI"/>
    </div>

    <div v-else class="w-full h-full grid grid-cols-2 flex-1">
      <div class="flex justify-center items-center flex-col">
        <p class="text-2xl font-bold">{{ victoryMessages[score] }}</p>
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
import victoryimg1 from "@/assets/games/1.jpeg";
import victoryimg2 from "@/assets/games/2.jpeg";
import victoryimg3 from "@/assets/games/3.jpeg";
import victoryimg4 from "@/assets/games/4.jpeg";
import victoryimg5 from "@/assets/games/5.jpeg";

import correctsound from "@/assets/games/correct.ogg";
import wrongsound from "@/assets/games/wrong.mp4";

import question1song from "@/assets/games/lyricgenius/1.mp4";
import question2song from "@/assets/games/lyricgenius/2.mp4";
import question3song from "@/assets/games/lyricgenius/3.mp4";
import question4song from "@/assets/games/lyricgenius/4.mp4";
import question5song from "@/assets/games/lyricgenius/5.mp4";
import titletheme from "@/assets/games/lyricgenius/titletheme.mp3";
import titlecover from "@/assets/games/lyricgenius/titlecover.jpeg";

const victoryMessages = ["That's embarrassing…", "Really?", "I mean...", "Could be worse", "Almost there", "You achieved true Isabelle level"]


const currQuestionI = ref(-1)
const currI = ref(0)
const score = ref(0);
const audio = new Audio();
audio.src = titletheme;
audio.play();

const sfx = new Audio();

const victoryImages = [victoryimg1, victoryimg1, victoryimg2, victoryimg3, victoryimg4, victoryimg5]
const questionSongs = [question1song, question2song, question3song, question4song, question5song];


const finalScoreAudio = [song1, song1, song2, song3, song4, song5]
const lyricsGeniusQuestions = [{
  options: ["I heard love is blind", "Love Story", "One last time", "Juno"],
  answer: "Love Story",
}, {
  options: ["Go your own way", "Non sense", "Landslide", "Friday night lights"],
  answer: "Landslide",

}, {
  options: ["Rich baby daddy", "Starboy", "Not like us", "I put a spell on you"],
  answer: "Rich baby daddy",
}, {
  options: ["Fuck me pumps", "Good 4 u", "7 rings", "You know Im no good"],
  answer: "Fuck me pumps",
}, {
  options: ["Boulevard of broken dreams", "Guys like you", "Black Dog", "Immigrant song"],
  answer: "Immigrant song",
}]

function onDown() {
  const newI = currI.value + 1;
  if (newI <= lyricsGeniusQuestions[0].options.length) currI.value = newI;
}

function onUp() {
  const newI = currI.value - 1;
  if (newI >= 0) currI.value = newI;
}

function onConfirm() {
  if (currQuestionI.value >= 0 && lyricsGeniusQuestions[currQuestionI.value].options[currI.value] == lyricsGeniusQuestions[currQuestionI.value].answer){
    score.value += 1;
  }

  currQuestionI.value++;
  audio.pause()
  if (currQuestionI.value >= lyricsGeniusQuestions.length) {
    audio.src = finalScoreAudio[score.value];
    audio.play();
  } else {
    audio.pause();
    audio.src = questionSongs[currQuestionI.value];
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