<template>
  <div class="w-full h-full bg-white flex justify-end items-center flex-col"  :style="{ backgroundImage: `url(${finalcover})`, backgroundSize: 'cover', backgroundPosition: 'center'}">
    <div class="bg-purple-300 flex justify-center flex-col items-center">
      <p class="text-2xl font-bold">You won!!!</p>
      <p class="text-xl">The PIN Code to eternal life is:</p>
      <p class="text-base font-bold">5683</p>
    </div>
  </div>
  <audio :src="finalmp3" autoplay/>
</template>

<script setup lang="ts">
import finalmp3 from "@/assets/final.m4a"
import finalcover from "@/assets/finalcover.jpg"
import {onMounted, onUnmounted} from "vue";
import {emitter} from "@/config.ts";

function onMenu() {
  emitter.emit("changeMenu", "main");
}

onMounted(() => {
  emitter.on("menu", onMenu);
})

onUnmounted(() => {
  emitter.off("menu", onMenu);
})


</script>

<style scoped>

</style>