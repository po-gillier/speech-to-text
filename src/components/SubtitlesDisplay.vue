<template>
  <div class="subtitles" :style="getBottom()">
    <p>{{ recognizedText }}</p>
  </div>
</template>
<script setup lang="ts">
import { defineProps } from 'vue'
const props = defineProps({
  recognizedText: { type: String, required: true },
  bot: { type: Number, default: 10 },
})

const getBottom = () => {
  return `bottom: ${props.bot}%`
}
</script>
<style scoped>
.subtitles {
  position: fixed;
  bottom: 10%;
  width: 100%;
  text-align: center;
  font-size: 2rem;
  color: white;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.8);
  background: rgba(0, 0, 0, 0.5);
  padding: 10px 20px;
  border-radius: 10px;
  box-sizing: border-box;
}
</style>
