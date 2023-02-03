<script setup>
const props = defineProps(["icons"]);
</script>
<template>
  <div v-for="icon in icons" :key="icon.src" :class="icon.class">
    <img :src="`/@assets/logos/${icon.src}`" />
  </div>
</template>
<style scoped>
@import "./SocialIcons.css";
</style>
