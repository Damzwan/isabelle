<template>
  <div class="h-6 w-full flex justify-between items-center"
       :class="{'bg-gradient-to-b from-[#54a3c2] to-[#2a5b85]': selected, 'bg-stone-200': disabled}">
    <p class="text-black text-base font-bold pl-1" :class="{'text-white': selected}">{{ text }}</p>

    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" v-if="selected && locked"
         class="size-5 text-white pr-1">
      <path stroke-linecap="round" stroke-linejoin="round"
            d="M16.5 10.5V6.75a4.5 4.5 0 1 0-9 0v3.75m-.75 11.25h10.5a2.25 2.25 0 0 0 2.25-2.25v-6.75a2.25 2.25 0 0 0-2.25-2.25H6.75a2.25 2.25 0 0 0-2.25 2.25v6.75a2.25 2.25 0 0 0 2.25 2.25Z"/>
    </svg>
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="size-5 text-white pr-1"
         v-else-if="selected">
      <path fill-rule="evenodd"
            d="M16.28 11.47a.75.75 0 0 1 0 1.06l-7.5 7.5a.75.75 0 0 1-1.06-1.06L14.69 12 7.72 5.03a.75.75 0 0 1 1.06-1.06l7.5 7.5Z"
            clip-rule="evenodd"/>
    </svg>


  </div>
</template>


<script setup lang="ts">
defineProps({
  text: String,
  selected: {
    type: Boolean,
    default: false
  },
  disabled: Boolean,
  locked: Boolean,
})

</script>

<style scoped>

</style>
