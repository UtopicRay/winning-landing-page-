<script setup lang="ts">
import {toRef, useTemplateRef} from "vue";

const props = defineProps({
  img: String,
  containerClass: String,
})
const imageRef = useTemplateRef('imageRef');

const onClick = toRef<boolean>(false)

function handleClick() {
  onClick.value = !onClick.value;
}

function MouseLeave() {
  imageRef.value.style.transform = '';
  console.log('leave')
}

function MouseEnter(e: MouseEvent) {
  const {width, height, top, left} = imageRef.value.getBoundingClientRect();
  const relativeX = (e.clientX - left) / width;
  const relativeY = (e.clientY - top) / height;
  const tiltX = (relativeX - 0.5) * 12;
  const tiltY = (relativeY - 0.5) * -12;
  imageRef.value.style.transform = `rotateX(${tiltX}deg) rotateY(${tiltY}deg) perspective(700px) scale3d(0.98,0.98,0.98)`;
}

</script>

<template>

  <div ref="imageRef"
       :class="`rounded-lg transition-all duration-300 z-10 ${onClick?'md:size-72 size-52':'md:size-14 size-12 bg-black'} ${containerClass}`"
       @click="handleClick" @mouseenter="MouseEnter" @mouseleave="MouseLeave">
    <img :src="props.img" alt="img-gallery"
         :class="`${onClick?' border-black border-2 rounded-lg size-full object-center object-cover z-10':'hidden'}`"/>
  </div>
</template>

<style scoped>

</style>