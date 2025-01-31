<template>
  <div class="w-full h-full bg-white flex flex-col">
    <MainBar title="Messages"/>
    <div class="w-full flex-1 flex flex-col">
      <MenuItem v-for="(item, i) in messages" :key="i" :text="item.text" :selected="currI == i"/>
    </div>
  </div>
</template>

<script setup lang="ts">

import {onMounted, onUnmounted, ref} from "vue";
import MainBar from "@/components/MainBar.vue";
import MenuItem from "@/components/MenuItem.vue";

import message1 from "@/assets/messages/1.mp4"
import message2 from "@/assets/messages/2.mp4"
import message3 from "@/assets/messages/3.mp4"
import message4 from "@/assets/messages/4.mp4"
import {emitter} from "@/config.ts";

const currI = ref(0)
const audio = new Audio();
const messages = [{text: "Message1.mp3", audio: message1}, {
  text: "Message2.mp3",
  audio: message2
}, {text: "Message3.mp3", audio: message3}, {text: "Message4.mp3", audio: message4}];

function onDown() {
  const newI = currI.value + 1;
  if (newI <= messages.length - 1) currI.value = newI;
}

function onUp() {
  const newI = currI.value - 1;
  if (newI >= 0) currI.value = newI;
}

function onConfirm() {
  audio.pause();
  audio.src = messages[currI.value].audio;
  audio.play();
}

function onMenu() {
  emitter.emit("changeMenu", "main");
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