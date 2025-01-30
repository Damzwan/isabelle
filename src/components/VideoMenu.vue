<template>
  <div class="w-full h-full">
    <div class="w-full h-full bg-white flex flex-col" v-show="!isShowingVideo">
      <MainBar title="Gallery"/>
      <div class="w-full flex-1">
        <swiper-container slides-per-view="2" :loop="true" class="flex justify-center items-center pt-6"
                          effect="coverflow"
                          :coverflowEffect="{
        stretch: 20,
        depth: 50,
        rotate: 50,
        slideShadows: false}" ref="swiper">
          <swiper-slide v-for="(item, i) in videos" :key="i">
            <img :src="item.cover" :alt="item.text" class="object-fill w-40 h-40"
                 :class="{'brightness-50': i === currI}">
            <div v-if="i == currI" class="absolute left-0 top-0 w-full h-full flex justify-center items-center">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                   stroke="currentColor" class="size-22 fill-white">
                <path stroke-linecap="round" stroke-linejoin="round"
                      d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.347a1.125 1.125 0 0 1 0 1.972l-11.54 6.347a1.125 1.125 0 0 1-1.667-.986V5.653Z"/>
              </svg>


            </div>
          </swiper-slide>

        </swiper-container>

        <p class="text-xl font-bold text-center pt-3">{{ videos[currI].text }}</p>

      </div>
    </div>

    <div class="w-full h-full bg-white flex flex-col" v-show="isShowingVideo">
      <MainBar :title="videos[currI].text"/>
      <div class="flex-1 w-full flex justify-center items-center">
        <video class="object-cover h-56" ref="video">
        </video>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">

import MainBar from "@/components/MainBar.vue";
import 'swiper/css';
import {register} from 'swiper/element/bundle';
import {onMounted, onUnmounted, ref} from "vue";
import {emitter} from "@/config.ts";
import cover1 from "@/assets/videos/1.png"
import cover2 from "@/assets/videos/2.png"
import cover3 from "@/assets/videos/3.png"
import cover4 from "@/assets/videos/4.png"
import cover5 from "@/assets/videos/5.png"
import cover6 from "@/assets/videos/6.png"
import coverhint from "@/assets/videos/videohint_cover.png"

import video1 from "@/assets/videos/1.mp4"
import video2 from "@/assets/videos/2.mp4"
import video3 from "@/assets/videos/3.mp4"
import video4 from "@/assets/videos/4.mp4"
import video5 from "@/assets/videos/5.mp4"
import video6 from "@/assets/videos/6.mp4"
import videohint from "@/assets/videos/videohint.mp4"


const isShowingVideo = ref(false)
const video = ref<HTMLVideoElement | undefined>()

const videos = [{text: "Geese are scary", cover: cover1, video: video1}, {
  text: "Facing your fears",
  cover: cover2,
  video: video2
}, {
  text: "Vivid tunnels",
  cover: cover3,
  video: video3
}, {
  text: "Trying to eat vegetables",
  cover: cover4,
  video: video4
},
  {
    text: "???",
    cover: coverhint,
    video: videohint
  },
  {
    text: "Lasting Impressions",
    cover: cover5,
    video: video5
  }, {
    text: "Annoying Isabelle",
    cover: cover6,
    video: video6
  }];
const currI = ref(0);
register();

const swiper = ref<any>()


function onDown() {
  if (isShowingVideo.value) return;
  swiper.value.swiper.slideNext();
  currI.value++;
  if (currI.value >= videos.length) currI.value = 0;

}

function onUp() {
  if (isShowingVideo.value) return;
  swiper.value.swiper.slidePrev();
  currI.value--;

  if (currI.value < 0) currI.value = videos.length - 1
}

function onConfirm() {
  if (!isShowingVideo.value) {
    emitter.emit("pauseaudio")

    video.value!.src = videos[currI.value].video;
    video.value!.play();
    isShowingVideo.value = true;
  } else {
    video.value!.paused ? video.value!.play() : video.value!.pause();
  }
}

function pausePlayVideo() {
  if (!isShowingVideo.value) return;
  video.value!.paused ? video.value!.play() : video.value!.pause();
}

function onMenu() {
  if (!isShowingVideo.value) {
    emitter.emit("changeMenu", "main")
  } else {
    video.value!.pause();
    isShowingVideo.value = false;
  }
}


onMounted(() => {
  emitter.on("down", onDown);
  emitter.on("up", onUp);
  emitter.on("confirm", onConfirm);
  emitter.on("menu", onMenu);
  emitter.on("pauseplay", pausePlayVideo);
})

onUnmounted(() => {
  emitter.off("down", onDown);
  emitter.off("up", onUp);
  emitter.off("confirm", onConfirm);
  emitter.off("menu", onMenu);
  emitter.off("pauseplay", pausePlayVideo);
})

</script>


<style scoped>

</style>