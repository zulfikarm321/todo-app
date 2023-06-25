<script setup>
import { useDark, useToggle } from '@vueuse/core';

const isDark = useDark();
const toggleDark = useToggle(isDark);
</script>

<template>
   <header class="flex justify-between mb-5">
      <h1 class="text-3xl text-white font-semibold tracking-[20px]">TODO</h1>
      <div class="theme-switcher cursor-pointer" @click="toggleDark()">
         <img v-if="isDark" src="images/icon-moon.svg" alt="moon icon" />
         <img v-else src="images/icon-sun.svg" alt="sun icon" />
      </div>
   </header>
</template>
