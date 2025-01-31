<template>
  <div class="h-4 bg-gradient-to-b from-white to-[#7f969f] flex justify-between items-center">
    <p class="text-sm font-semibold pl-1 truncate w-full whitespace-nowrap overflow-hidden">
      {{ musicPlaying ? musicPlayingTitle : title }}</p>
    <div class="flex items-center justify-between pr-1">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-4 fill-[#3dadcd]" v-if="!musicPlaying">
        <path fill-rule="evenodd"
              d="M4.5 5.653c0-1.427 1.529-2.33 2.779-1.643l11.54 6.347c1.295.712 1.295 2.573 0 3.286L7.28 19.99c-1.25.687-2.779-.217-2.779-1.643V5.653Z"
              clip-rule="evenodd"/>
      </svg>
      <svg xmlns="http://www.w3.org/2000/svg" fill="#3dadcd" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-4 fill-[#3dadcd] stroke-[#3dadcd]" v-else>
        <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 5.25v13.5m-7.5-13.5v13.5" />
      </svg>


      <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px"
           class="fill-green-600"
           fill="#e8eaed">
        <path
            d="M200-280q-17 0-28.5-11.5T160-320v-80H80v-160h80v-80q0-17 11.5-28.5T200-680h640q17 0 28.5 11.5T880-640v320q0 17-11.5 28.5T840-280H200Z"/>
      </svg>

    </div>
  </div>
</template>

<script setup lang="ts">

import {musicPlaying, musicPlayingTitle} from "../store.ts";

defineProps({
  title: String,
})

</script>

<style scoped>

</style>