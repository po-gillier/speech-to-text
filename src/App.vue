<template>
  <div id="app">
    <h1>Live Subtitling Application auto deployed</h1>
    <SubtitlesDisplay :recognizedText="text" :bot="10" />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import SubtitlesDisplay from './components/SubtitlesDisplay.vue'

// Reactive variables
const text = ref('')
const finals = ref('')
const notFinal = ref('')

// Speech recognition setup
const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition

if (SpeechRecognition) {
  const speechRecognition = new SpeechRecognition()
  speechRecognition.lang = 'fr-FR'
  speechRecognition.continuous = true
  speechRecognition.interimResults = true
  speechRecognition.maxAlternatives = 1

  speechRecognition.onresult = handleSpeechResult
  speechRecognition.onerror = handleSpeechError

  speechRecognition.start()
} else {
  console.error('Speech recognition not supported in this browser.')
}

// Handlers
function handleSpeechResult(event: SpeechRecognitionEvent) {
  finals.value = ''
  notFinal.value = ''

  for (let i = 0; i < event.results.length; ++i) {
    const result = event.results[i]
    const transcript = result[0].transcript

    if (result[0].confidence > 0.4) {
      if (result.isFinal) {
        finals.value += transcript
      } else {
        notFinal.value = transcript
      }
    }
  }

  text.value = `${finals.value} ${notFinal.value}`
  console.log('Recognized text:', text.value)
}

function handleSpeechError(event: SpeechRecognitionError) {
  console.error('Speech recognition error:', event.error)
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
