<template>
  <div class="w-full h-full grid grid-cols-2">
    <div class="w-full h-full bg-white">
      <MainBar title="iPod"/>

      <div>
        <MenuItem v-for="(item, i) in menus" :key="i" :text="item.title" :selected="i == currI" :locked="item.locked"
                  :disabled="isDisabled(i)"/>
      </div>

    </div>
    <div class="w-full h-full relative">
      <img :src="nirvana" alt="" class="h-full" />
    </div>

    <!-- Dark Background Overlay -->
    <div class="absolute inset-0 z-40 bg-black/50" v-show="showLockDialog"></div>

    <!-- Dialog Box -->
    <div
        class="absolute inset-0 z-50 flex justify-center left-[2.5%] w-[95%] bg-white h-[90%] top-[5%] rounded-md"
        v-show="showLockDialog"
    >
      <div class="p-4 space-y-4">
        <!-- Question -->
        <p class="text-sm font-bold">{{ menus[currI].question }}</p>

        <!-- Text Input -->
        <input
            type="text"
            v-model="dialogAnswer"
            class="w-full px-4 py-1 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:outline-none"
            placeholder="Type your answer here..."
        />

        <!-- Actions -->
        <div class="flex justify-between space-x-2">
          <button
              @click="() => {
                dialogIndex = 0
                onConfirm()
              }"
              class="px-4 py-2 w-full text-sm font-medium rounded-md"
              :class="{'text-gray-600 bg-gray-100': dialogIndex == 1, 'text-white bg-blue-600': dialogIndex == 0}"
          >
            Cancel
          </button>
          <button
              @click="() => {
                dialogIndex = 1
                onConfirm()
              }"
              class="px-4 py-2 w-full text-sm font-medium bg-blue-600 rounded-md"
              :class="{'text-gray-600 bg-gray-100': dialogIndex == 0, 'text-white bg-blue-600': dialogIndex == 1}"
          >
            Confirm
          </button>
        </div>
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

const showLockDialog = ref(false);
const lockText = ref("");
import errorsfx from "@/assets/error.m4a"
import goodsfx from "@/assets/reallygood.m4a"

const dialogIndex = ref(1);

const errorAudio = new Audio(errorsfx);

const menus = ref([
  {title: "Music", locked: false},
  {title: "Pictures", locked: false},
  {title: "Videos", locked: false},
  {
    title: "Extras", locked: !localStorage.getItem("Extras"), question: "In what year did time align,\n" +
        "To spark the song that now is mine?\n" +
        "Turn the handle, let it play,\n" +
        "And find the answer in its sway.", answer: "1960"
  },
  {
    title: "Games", locked: !localStorage.getItem("Games"), question: "In the harbor’s glow, where music’s stacked,\n" +
        "Who rocks the scene with rhythm packed?", answer: "boysattheback"
  },
  {
    title: "Messages",
    locked: !localStorage.getItem("Messages"),
    question: "The names I give aren’t quite the same,\n" +
        "They’re cloaked in jest, a playful game.\n" +
        "Take the lead of those who sing,\n" +
        "Not my masks—it’s their true ring.\n" +
        "Piece them together, one by one,\n" +
        "And you’ll unveil where it’s all spun.",
    answer: "norman"
  },
  {
    title: "Final", locked: !localStorage.getItem("Final"), question: "Scattered pieces, a tangled view,\n" +
        "Fit them right to find the clue.\n" +
        "A name that hints at what you do—\n" +
        "What’s the puzzle calling you?", answer: "gracie"
  }
]);
const currI = ref(0);
const dialogAnswer = ref("");

function isDisabled(i: number): boolean {
  return menus.value[i].locked && menus.value.slice(0, i).some(item => item.locked);
}

function onDown() {
  if (showLockDialog.value) {
    dialogIndex.value = 1;
  } else {
    const newI = currI.value + 1;
    if (newI <= menus.value.length - 1 && !isDisabled(newI)) currI.value = newI;
  }
}

function onUp() {
  if (showLockDialog.value) {
    dialogIndex.value = 0;
  } else {
    const newI = currI.value - 1;
    if (newI >= 0) currI.value = newI;
  }
}

function onConfirm() {
  if (showLockDialog.value) {
    if (dialogIndex.value == 0) showLockDialog.value = false;
    else if (dialogAnswer.value.toLowerCase() == menus.value[currI.value].answer) {
      menus.value[currI.value].locked = false;
      showLockDialog.value = false;
      localStorage.setItem(menus.value[currI.value].title, "shaba");
      errorAudio.src = goodsfx;
      console.log("nan")
      errorAudio.play()
    } else {
      errorAudio.src = errorsfx;
      errorAudio.play()
    }
    dialogIndex.value = 1;
    dialogAnswer.value = "";
  } else if (menus.value[currI.value].locked) {
    lockText.value = menus.value[currI.value].question!;
    showLockDialog.value = true;
  } else emitter.emit("changeMenu", menus.value[currI.value].title.toLowerCase());
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