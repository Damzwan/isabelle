<template>
  <div class="w-full h-full">
    <component :is="gameToShow" v-if="gameToShow"/>
    <div class="w-full h-full grid grid-cols-2" v-else>
      <div class="w-full h-full bg-white">
        <MainBar title="Games"/>

        <div>
          <MenuItem v-for="(item, i) in menus" :key="i" :text="item" :selected="i == currI"/>
        </div>

      </div>
      <div class="w-full h-full">
        <img :src="nirvana" alt="" class="h-full">
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">

import nirvana from "@/assets/nirvana.jpg";
import MenuItem from "@/components/MenuItem.vue";
import {emitter} from "@/config.ts";
import {onMounted, onUnmounted, ref} from "vue";
import MainBar from "@/components/MainBar.vue";
import TriviaGame from "@/components/TriviaGame.vue";
import LyricGenius from "@/components/LyricGenius.vue";

const menus = ref(["Trivia Master", "Lyric Genius"]);
const gameMenus = [TriviaGame, LyricGenius];
const gameToShow = ref<any>();

const currI = ref(0);

function onDown() {
  if (gameToShow.value) return;
  const newI = currI.value + 1;
  if (newI <= menus.value.length - 1) currI.value = newI;
}

function onUp() {
  if (gameToShow.value) return;
  const newI = currI.value - 1;
  if (newI >= 0) currI.value = newI;
}

function onConfirm() {
  if (gameToShow.value) return;
  gameToShow.value = gameMenus[currI.value];
}

function onMenu() {
  if (!gameToShow.value) {
    emitter.emit("changeMenu", "main");
  } else {
    gameToShow.value = undefined
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