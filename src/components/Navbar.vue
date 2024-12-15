<script setup lang="ts">
import {toRef, useTemplateRef} from "vue";

const navItem = toRef<string[]>(['Nexus', 'Vault', 'Prologue', 'About', 'Contact'])
const indicatorActive=toRef<boolean>(false);
const audioRef=useTemplateRef('audioRef');

function handleAudio(){
  indicatorActive.value = !indicatorActive.value;
  if(indicatorActive.value){
    audioRef.value?.play();
  }
  else
    audioRef.value?.pause();
}

</script>
<template>
  <div class="fixed px-6 py-2 z-50 w-full">
    <nav class="flex justify-between items-center size-full floating-nav px-6 border-black">
      <div class="flex items-center gap-x-4">
        <img src="/img/logo.png" alt="logo_album"/>
        <button class="group flex gap-2 z-10 rounded-full bg-blue-50 px-7 py-3 w-fit overflow-hidden">
          <span>Products</span>
          <svg xmlns="http://www.w3.org/2000/svg" width="14" height="24" viewBox="0 0 24 24" fill=""
               stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
               class="icon icon-tabler icons-tabler-outline icon-tabler-location">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
            <path d="M21 3l-6.5 18a.55 .55 0 0 1 -1 0l-3.5 -7l-7 -3.5a.55 .55 0 0 1 0 -1l18 -6.5"/>
          </svg>
        </button>
      </div>
      <div class="flex items-center h-full">
        <div class="md:block hidden" v-for="nav in navItem" :key="nav">
          <a :href="`#${nav.toLowerCase()}`" class="nav-hover-btn">
            {{ nav }}
          </a>
        </div>
        <button class="ml-10 space-x-0.5 flex items-center" @click="handleAudio()">
          <audio ref="audioRef" class="hidden" src="/audio/loop.mp3"  autoplay loop></audio>
          <div v-for="bar in [1,2,3,4]" :key="bar">
            <div :class="`indicator-line ${indicatorActive&&'active'}`" :style="`animation-delay: ${bar*0.1}s`"></div>
          </div>
        </button>
      </div>
    </nav>
  </div>
</template>