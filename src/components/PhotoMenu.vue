<template>
  <div class="w-full h-full bg-white flex flex-col">
    <MainBar title="Gallery"/>
    <div class="w-full flex-1">
      <swiper-container slides-per-view="1" initialSlide="0" :loop="true"
                        class="flex justify-center items-center h-full"
                        ref="swiper">
        <swiper-slide v-for="item in images" class="flex justify-center items-center">
          <img :src="item" :alt="item" class="object-fill h-56">
        </swiper-slide>

      </swiper-container>
    </div>
  </div>
</template>

<script setup lang="ts">

import MainBar from "@/components/MainBar.vue";
import img1 from "@/assets/pictures/1.jpeg"
import img2 from "@/assets/pictures/2.jpeg"
import img3 from "@/assets/pictures/3.jpeg"
import img4 from "@/assets/pictures/4.jpeg"
import img5 from "@/assets/pictures/5.jpeg"
import img6 from "@/assets/pictures/6.jpeg"
import img7 from "@/assets/pictures/7.jpeg"
import img8 from "@/assets/pictures/8.jpeg"
import img9 from "@/assets/pictures/9.jpeg"
import img10 from "@/assets/pictures/10.jpeg"
import img11 from "@/assets/pictures/11.jpeg"
import img12 from "@/assets/pictures/12.jpeg"
import img13 from "@/assets/pictures/13.jpeg"
import img14 from "@/assets/pictures/14.jpeg"
import img15 from "@/assets/pictures/15.jpeg"
import img16 from "@/assets/pictures/16.jpeg"
import img17 from "@/assets/pictures/17.jpeg"
import hint from "@/assets/pictures/hintimage.jpeg"
import 'swiper/css';
import {register} from 'swiper/element/bundle';
import {onMounted, onUnmounted, ref} from "vue";
import {emitter} from "@/config.ts";

const images = [img1, img2, img3, img4, img5, img6, img7, hint, img8, img9, img10, img11, img12, img13, img14, img15, img16, img17];
const currI = ref(0);
register();

const onSlideChange = (e: any) => {
  if (swiper.value) currI.value = swiper.value.swiper.activeIndex;
}

const swiper = ref<any>()


function onDown() {
  swiper.value.swiper.slideNext();

}

function onUp() {
  swiper.value.swiper.slidePrev();
}

function onMenu() {
  emitter.emit("changeMenu", "main");
}


onMounted(() => {
  emitter.on("down", onDown);
  emitter.on("up", onUp);
  emitter.on("menu", onMenu);

})

onUnmounted(() => {
  emitter.off("down", onDown);
  emitter.off("up", onUp);
  emitter.off("menu", onMenu);
})

</script>


<style scoped>

</style>