<script setup lang="ts">
import {gsap} from "gsap";
import {onMounted, useTemplateRef} from "vue";

const props = defineProps({
  title: String,
  containerClass: String,
})
const separator: string = "<br/>"
const lines: string[] = props.title?.split(separator);
const currentRef = useTemplateRef('animated-title');

onMounted(() => {
  const ctx = gsap.context(() => {
    const animateText = gsap.timeline({
      scrollTrigger: {
        trigger:currentRef.value,
        start: '100 bottom',
        end: 'center bottom',
        toggleActions: 'play none none reverse'
      }
    })
    gsap.to('.animated-word', {
      transform: 'rotateY(0deg) rotateX(0deg) translate3d(0,0,0)',
      opacity: 1,
      ease: 'power2.inOut',
      stagger: 0.02
    });
  },currentRef.value)
  return () => ctx.revert()
})
</script>

<template>
  <div ref="animated-title" :class="`${props.containerClass} animated-title w-full`" >
    <div class="flex-center flex-wrap max-w-full gap-2" v-for="(line,index) in lines" :key="index">
      <span class="animated-word" v-for="(word,index) in line.split(' ')" :key="index">{{ word }}</span>
    </div>
  </div>

</template>

<style scoped>

</style>