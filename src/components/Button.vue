<script setup lang="ts">

const props=defineProps({
  containerClass:String,
  title:String,
  })
</script>

<template>
  <button :class="`group flex gap-2 z-10 rounded-full px-7 py-3 w-fit overflow-hidden cursor-pointer ${props.containerClass}`">
    <slot name="leftIcon"/>
    <span>{{props.title}}</span>
    <slot name="rightIcon"/>
  </button>
</template>

<style scoped>

</style>