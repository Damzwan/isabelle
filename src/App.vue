<template>
  <div class="fixed left-0 top-0">
    <div class="relative overflow-hidden">
      <img :src="ipod" alt="ipod" class="top-0 w-screen max-w-[430px]"/>
      <div
          class="w-[81.5%] max-w-[430px] h-[36.6%] left-[9.5%] top-[4.5%] bg-red-200 z-10 absolute left-0 top-0 rounded-lg">
        <component :is="menus[currMenu]"/>
      </div>

      <div class="absolute bottom-[12%] w-[63%] h-[36.3%] left-[19%] opacity-50">
        <div class="w-full h-full relative">
          <button class="w-[50%] h-[31%] left-[25%] absolute top-0 touch-none"
                  @click="emitter.emit('menu')"/>
          <button class="w-[50%] h-[31%] left-[25%] bottom-0 absolute touch-none"
                  @click="emitter.emit('pauseplay')"/>
          <button class=" w-[31%] h-[35%] left-[0] top-[32%] absolute touch-none"
                  @click="emitter.emit('up')"/>
          <button class="w-[31%] h-[35%] left-[69%] top-[32%] absolute touch-none"
                  @click="emitter.emit('down')"/>
          <button class="w-[35%] h-[36%] left-[32.5%] top-[32%] absolute touch-none"
                  @click="emitter.emit('confirm')"/>
        </div>
      </div>
    </div>

    <button class="absolute bottom-64 left-5 bg-red-300" @click="showHelp = !showHelp">HELP</button>

    <div class="flex flex-col p-3 absolute left-0 bottom-0 left-0 bg-cyan-200 z-10" v-if="showHelp">
      <p class="text-xl font-bold">Use the buttons of the IPOD to navigate:</p>
      <p class="text-base flex items-center">
  <span class="inline-flex">
    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
         class="size-6">
      <path stroke-linecap="round" stroke-linejoin="round"
            d="M21 16.811c0 .864-.933 1.406-1.683.977l-7.108-4.061a1.125 1.125 0 0 1 0-1.954l7.108-4.061A1.125 1.125 0 0 1 21 8.689v8.122ZM11.25 16.811c0 .864-.933 1.406-1.683.977l-7.108-4.061a1.125 1.125 0 0 1 0-1.954l7.108-4.061a1.125 1.125 0 0 1 1.683.977v8.122Z"/>
    </svg>
  </span>
        <span class="ml-1">Navigate left or up</span>
      </p>

      <p class="text-base flex items-center">
  <span class="inline-flex">
<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"
     class="size-6">
  <path stroke-linecap="round" stroke-linejoin="round"
        d="M3 8.689c0-.864.933-1.406 1.683-.977l7.108 4.061a1.125 1.125 0 0 1 0 1.954l-7.108 4.061A1.125 1.125 0 0 1 3 16.811V8.69ZM12.75 8.689c0-.864.933-1.406 1.683-.977l7.108 4.061a1.125 1.125 0 0 1 0 1.954l-7.108 4.061a1.125 1.125 0 0 1-1.683-.977V8.69Z"/>
</svg>

  </span>
        <span class="ml-1">Navigate right or down</span>
      </p>

      <p class="text-base flex items-center">
  <span class="inline-flex">
    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
  <path stroke-linecap="round" stroke-linejoin="round" d="M21 7.5V18M15 7.5V18M3 16.811V8.69c0-.864.933-1.406 1.683-.977l7.108 4.061a1.125 1.125 0 0 1 0 1.954l-7.108 4.061A1.125 1.125 0 0 1 3 16.811Z" />
</svg>

  </span>
        <span class="ml-1">Play or Pause a song</span>
      </p>

      <p class="text-base">MENU Go back to the previous menu</p>
      <p class="text-base">Middle button: confirm action</p>

      <button class="bg-red-200" @click="showHelp=false">Close Me</button>

    </div>

  </div>
</template>


<script setup lang="ts">
import ipod from "@/assets/ipod.png"
import MainMenu from "@/components/MainMenu.vue";
import {ref} from "vue";
import {emitter} from "@/config.ts";
import MusicMenu from "@/components/MusicMenu.vue";
import VideoMenu from "@/components/VideoMenu.vue";
import PhotoMenu from "@/components/PhotoMenu.vue";
import GamesMenu from "@/components/GamesMenu.vue";

import song1 from "@/assets/songs/1.mp3";
import song2 from "@/assets/songs/2.mp3";
import song3 from "@/assets/songs/3.mp3";
import song4 from "@/assets/songs/4.mp3";
import song5 from "@/assets/songs/5.mp3";
import song6 from "@/assets/songs/6.mp3";
import song7 from "@/assets/songs/7.mp3";
import song8 from "@/assets/songs/8.mp3";
import song9 from "@/assets/songs/9.mp3";
import secretsong from "@/assets/songs/music_secret.mp4";
import IntroMenu from "@/components/IntroMenu.vue";
import ExtrasMenu from "@/components/ExtrasMenu.vue";
import MessagesMenu from "@/components/MessagesMenu.vue";
import FinalPage from "@/components/FinalPage.vue";
import {musicPlaying, musicPlayingTitle} from "@/store.ts";

const menus: any = {
  "main": MainMenu,
  "music": MusicMenu,
  "pictures": PhotoMenu,
  "videos": VideoMenu,
  "games": GamesMenu,
  "intro": IntroMenu,
  "extras": ExtrasMenu,
  "messages": MessagesMenu,
  "final": FinalPage
};
const currMenu = ref(localStorage.getItem("intro") ? "main" : "intro");

const songs = [song1, song2, song3, song4, song5, song6, secretsong, song7, song8, song9];

function changeMenu(menu: any) {
  currMenu.value = menu;
}

const showHelp = ref(false )

function pausePlay() {
  if (currMenu.value == "videos") return
  audio.paused ? audio.play() : audio.pause();
  musicPlaying.value = !audio.paused
}

const audio = new Audio('');

emitter.on("changeMenu", changeMenu);
emitter.on("playSong", (song: any) => {
  audio.pause();
  const matchingSong = songs[song];
  audio.src = matchingSong;
  audio.play();
  musicPlaying.value = true
});
emitter.on("pauseplay", pausePlay);
emitter.on("pauseaudio", () => {
  if (!audio.paused) audio.pause()
  musicPlaying.value = false
});

</script>

<style scoped>

</style>
