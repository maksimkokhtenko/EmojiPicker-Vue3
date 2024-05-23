<script setup>
import EmojiPicker from './components/EmojiPicker/EmojiPicker.vue'
</script>

<template>

  <main>
    <h1>EmojiPicker Vue3 Demo</h1>
    <br>
    <EmojiPicker :enableSearch="true" :enableFrequentlyUsed="true"/>
  </main>
</template>
