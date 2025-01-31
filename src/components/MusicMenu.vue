<template>
  <div class="w-full h-full bg-white flex flex-col">
    <MainBar title="Cover Flow"/>
    <div class="w-full flex-1">
      <swiper-container slides-per-view="2" :loop="true" class="flex justify-center items-center pt-6"
                        effect="coverflow"
                        :coverflowEffect="{
        stretch: 20,
        depth: 50,
        rotate: 50,
        slideShadows: false}" ref="swiper">
        <swiper-slide v-for="(item, i) in songs">
          <img :src="item.cover" :alt="`song-${i}`" class="object-fill w-40 h-40"
               :class="{'brightness-50': i === currI}">
          <div v-if="i == currI && (!musicPlaying || (musicPlaying && musicPlayingTitle!=item.text))"
               class="absolute left-0 top-0 w-full h-full flex justify-center items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                 stroke="currentColor" class="size-22 fill-white">
              <path stroke-linecap="round" stroke-linejoin="round"
                    d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.347a1.125 1.125 0 0 1 0 1.972l-11.54 6.347a1.125 1.125 0 0 1-1.667-.986V5.653Z"/>
            </svg>
          </div>
          <div v-else-if="i === currI"
               class="absolute left-0 top-0 w-full h-full flex justify-center items-center">
            <svg xmlns="http://www.w3.org/2000/svg" fill="#3dadcd" viewBox="0 0 24 24" stroke-width="1.5"
                 stroke="currentColor"  class="size-22 fill-white stroke-white">
              <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 5.25v13.5m-7.5-13.5v13.5"/>
            </svg>
          </div>

        </swiper-slide>

      </swiper-container>

      <p class="text-xl font-bold text-center pt-4">{{ songs[currI].text }}</p>

    </div>
  </div>
</template>

<script setup lang="ts">

import MainBar from "@/components/MainBar.vue";
import 'swiper/css';
import {register} from 'swiper/element/bundle';
import {onMounted, onUnmounted, ref} from "vue";
import {emitter} from "@/config.ts";

import cover1 from "@/assets/songs/cover1.jpg"
import cover2 from "@/assets/songs/cover2.jpg"
import cover3 from "@/assets/songs/cover3.jpg"
import cover4 from "@/assets/songs/cover4.jpg"
import cover5 from "@/assets/songs/cover5.jpg"
import cover6 from "@/assets/songs/cover6.jpg"
import cover7 from "@/assets/songs/cover7.jpg"
import cover8 from "@/assets/songs/cover8.jpg"
import cover9 from "@/assets/songs/cover9.jpg"
import secret_cover from "@/assets/songs/cover_secret.jpg"
import {musicPlaying, musicPlayingTitle} from "@/store.ts";


const songs = [{text: "Rich baby daddy", cover: cover1}, {
  text: "Rock and Roll",
  cover: cover2,
}, {
  text: "The chain",
  cover: cover3,
}, {
  text: "Fuck me pumps",
  cover: cover4,
}, {
  text: "Nonsense",
  cover: cover5,
}, {
  text: "Silver springs",
  cover: cover6,
},
  {
    text: "Crushed balls",
    cover: secret_cover,
  }, {
    text: "Snow angel",
    cover: cover7,
  }, {
    text: "Bombshell",
    cover: cover8,
  }, {
    text: "Since I have been loving you",
    cover: cover9,
  }];

const currI = ref(0);
register();

const swiper = ref<any>()


function onDown() {
  swiper.value.swiper.slideNext();
  currI.value++;
  if (currI.value >= songs.length) currI.value = 0;
}

function onUp() {
  swiper.value.swiper.slidePrev();
  currI.value--;
  if (currI.value < 0) currI.value = songs.length - 1

}

function onMenu() {
  emitter.emit("changeMenu", "main");
}

function onConfirm() {
  if (musicPlayingTitle.value == songs[currI.value].text){
    emitter.emit("pauseplay", currI.value);

  }
  else{
    musicPlayingTitle.value = songs[currI.value].text;
    emitter.emit("playSong", currI.value);
  }
}

onMounted(() => {
  emitter.on("down", onDown);
  emitter.on("up", onUp);
  emitter.on("confirm", onConfirm);
  emitter.on("menu", onMenu);
})

onUnmounted(() => {
  emitter.off("down", onDown);
  emitter.off("up", onUp);
  emitter.off("confirm", onConfirm);
  emitter.off("menu", onMenu);
})

</script>


<style scoped>

</style>