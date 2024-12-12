<script setup lang="ts">
import {onMounted, toRef, useTemplateRef, watch} from "vue";
import '/public/index.css';
import gsap from "gsap"
/*import {ScrollTrigger} from "gsap/ScrollTrigger";

gsap.registerPlugin( ScrollTrigger)*/

const isLoading = toRef<boolean>(true);
const hasClicked = toRef<boolean>(false);
const currentIndex = toRef<number>(1);
const nextIndex = toRef<number>(2);
const loadedVideos = toRef<number>(0);
const currentVideo = toRef<string>(`/videos/hero-${currentIndex.value}.mp4`);
const nextVideo = toRef<string>(`/videos/hero-${nextIndex.value}.mp4`)
const refNextVideo = useTemplateRef<HTMLVideoElement>('next-video')

function handleClick() {
  hasClicked.value = true;
  currentIndex.value++;
  nextIndex.value++;
  if (currentIndex.value > 4) {
    currentIndex.value = 1;
  }
  if (nextIndex.value > 4) {
    nextIndex.value = 1;
  }
  currentVideo.value = `/videos/hero-${currentIndex.value}.mp4`;
  nextVideo.value = `/videos/hero-${nextIndex.value}.mp4`;
}

onMounted(() => {
  gsap.set('#video-frame', {
    clipPath: 'polygon(16% 0%, 79% 0%, 97% 79%, 0% 100%)',
  })
  gsap.from('#video-frame', {
    clipPath: 'polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%)',
    borderRadius: '50px 50px, 0px 50%, 0 50%, 0 50%',
    duration:1,
    ease:'power1.inOut',
    scrollTrigger: {
      scrub:true,
    },
  })


})

watch(nextIndex, () => {
  const cxt = gsap.context(() => {
    gsap.set('#current-video', {
      visibility: 'visible',
    })
    gsap.to('#current-video', {
      transformOrigin: 'center center',
      width: '100%',
      height: '100%',
      scale: 1,
      duration: 1,
      ease: 'power1.inOut',
      onStart: () => refNextVideo.value?.play(),
    })
    gsap.from(refNextVideo.value, {
      transformOrigin: 'center center',
      scale: 0,
      duration: 1.5,
      ease: 'power1.inOut',
    })
  })
  setInterval(() => {
   cxt.revert()
  }, 2000)
})

</script>
<template>
  <div class="relative h-dvh w-screen overflow-x-hidden">
    <div id="video-frame" class="relative z-10 h-dvh w-screen overflow-hidden rounded-lg bg-blue-75">
      <div>
        <div
            class="mask-clip-path absolute-center absolute z-50 size-64 overflow-hidden rounded-lg opacity-0 hover:opacity-100 duration-200 transition-all ease-in scale-50 hover:scale-100">
          <div class="origin-center rounded-lg" @click="handleClick">
            <video ref="next-video" muted id="next-video" class="size-64 origin-center scale-150"
                   :src=nextVideo></video>
          </div>
        </div>
        <video loop muted autoplay id="current-video"
               class="size-64 invisible z-20 origin-center absolute absolute-center object-center object-cover"
               :src="currentVideo"></video>
        <video loop muted autoplay class="size-full object-cover object-center absolute top-0"
               :src="currentVideo"></video>
        <h1 class="absolute bottom-3 right-3 special-font text-blue-75 hero-heading z-40">G<b>A</b>MING</h1>

        <div class="absolute top-0 left-0 size-full z-40">
          <div class="px-5 mt-24 flex flex-col gap-y-2">
            <h1 class="special-font text-blue-100 hero-heading">redifi<b>n</b>e</h1>
            <p class="max-w-64 text-blue-100">Enter the Metagame
              <br>Unleash the Play Economy</p>
            <button class="group flex gap-2 z-10 rounded-full bg-yellow-300 px-7 py-3 w-fit overflow-hidden">
              <svg xmlns="http://www.w3.org/2000/svg" width="14" height="24" viewBox="0 0 24 24" fill=""
                   stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
                   class="icon icon-tabler icons-tabler-outline icon-tabler-location">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <path d="M21 3l-6.5 18a.55 .55 0 0 1 -1 0l-3.5 -7l-7 -3.5a.55 .55 0 0 1 0 -1l18 -6.5"/>
              </svg>
              <span>Watch Trailer</span>
            </button>
          </div>
        </div>
      </div>
    </div>
    <h1 class="absolute bottom-3 right-3 special-font hero-heading">G<b>A</b>MING</h1>
  </div>

</template>
