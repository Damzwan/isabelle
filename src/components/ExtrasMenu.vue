<template>
  <div class="w-full h-full">
    <div class="w-full h-full bg-white flex flex-col" v-if="selectedItem == -1">
      <MainBar title="Extras"/>
      <div class="flex-1 w-full h-full">
        <MenuItem v-for="(item, i) in items" :key="i" :text="item" :selected="currI == i"/>
      </div>
    </div>
    <div class="w-full h-full bg-white flex justify-center items-center relative" v-else-if="selectedItem == 0">
      <video :src="extravideo" autoplay class="h-[95%]"/>
    </div>

    <div class="w-full h-full bg-white p-4 overflow-scroll" id="container" v-show="selectedItem == 1">
      <p class="text-center font-semibold text-base handlee-regular" id="textEL">

      </p>
    </div>
  </div>

</template>

<script setup lang="ts">

import MainBar from "@/components/MainBar.vue";
import MenuItem from "@/components/MenuItem.vue";
import {onMounted, onUnmounted, ref} from "vue";
import {emitter} from "@/config.ts";
import Typed from 'typed.js';
import extravideo from "@/assets/extras/extra.mp4"

const currI = ref(0)
const items = ["Weird ass video", "Super secret extra 😈"]
const selectedItem = ref(-1)
let interval;
let typed;

function onDown() {
  const newI = currI.value + 1;
  if (newI <= items.length - 1) currI.value = newI;
}

function onUp() {
  const newI = currI.value - 1;
  if (newI >= 0) currI.value = newI;
}

function onConfirm() {
  selectedItem.value = currI.value;
  if (currI.value == 1) {
    typed = new Typed('#textEL', {
      strings: [
        "Dear Isabelle,<br><br>" +

        "If you are reading this, that means you have finally received my gift and already completed part of the puzzle I made for you. " +
        "I hope you've enjoyed it so far and that it wasn’t too easy for you.<br><br>" +

        "As I mentioned in my other letter, I spent a long time thinking about what to give you, and I decided on this. " +
        "I truly hope you like it. I certainly enjoyed making it for you, even though it made me cry on several occasions.<br><br>" +

        "Going through my photos and videos of us brought me back to all the memories we’ve shared—the times I annoyed you, " +
        "the places we explored together, and all the fun dates we went on.<br><br>" +

        "No matter what happens in the future, I will always cherish the time we spent together, " +
        "and I hope we can create even more memories in the future.<br><br>" +

        "One memory that keeps coming back to me is our date in the city. We went to that photobooth place on George Street, " +
        "took some adorable pictures, and then visited that arcade (the name escapes me) where we won that Minnie and Mickey Mouse toy. " +
        "That was a good day—I was really happy then.<br><br>" +

        "I don’t think I’ve ever told you this, but I often think about the time we were sitting on your couch, " +
        "and you told me how your brain works—how you analyze words with numbers, turning people's names into special numbers. " +
        "If they don’t fit into one, it’s a bad sign. (I probably can’t explain it properly, but I assume you know what I mean.)<br><br>" +

        "That moment was special to me. I remember thinking: <em>My girl is so adorable and weird… but mainly adorable.</em> " +
        "I was so happy you shared that little quirky fact about yourself.<br><br>" +

        "I could go on and on about all the beautiful moments we’ve shared, but I’ll leave it at this—thank you, Isabelle. " +
        "I will never forget those times.<br><br>" +

        "These last couple of months have been really hard for many reasons that I won’t go into here, " +
        "because I don’t want to turn this into something sad. But I really miss you.<br><br>" +

        "I miss your joyful spirit (even if you might disagree with me on that). " +
        "I miss your lovely personality. I miss hanging out with you. I miss sleeping next to you.<br><br>" +

        "But more than anything… I miss my best friend.<br><br>" +

        "I hope you achieve all your goals this year. I hope your band keeps growing and reaches the level you dream of. " +
        "I know you’ll graduate with amazing grades, because <em>Isabelle is Isabelle.</em> " +
        "And above all, I truly hope you find happiness in your life.<br><br>" +

        "Whatever happens, you will always have a special place in my heart.<br><br>" +

        "<strong>I love you very much, Isabelle.</strong><br><br>" +

        "I hope you have a lovely day, and good luck with the rest of the puzzle.<br><br>" +

        "Yours truly,<br>" +
        "Robi"
      ],
      typeSpeed: 50,
      backSpeed: 25,
      showCursor: false,
    });
    interval = setInterval(function () {
      let container = document.getElementById('container');
      container.scrollTop = container.scrollHeight; // Auto-scroll to the bottom
    }, 1000)
  }
}

function onMenu() {
  clearInterval(interval);
  typed.destroy()
  if (selectedItem.value === -1) {
    emitter.emit("changeMenu", "main");
  } else selectedItem.value = -1
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
.handlee-regular {
  font-family: "Handlee", serif;
  font-weight: 400;
  font-style: normal;
}

</style>