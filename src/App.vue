<template>
  <div class="w-3/5 m-auto h-screen flex items-center justify-center flex-col">
    <button class="btn btn-primary" @click="toggleMicrophone" ref="recordBtn">start recording</button>

    <!-- Transcript -->
    <div class="transcript my-4 font-bold text-justify" v-text="transcript"></div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const transcript = ref('')
const isRecording = ref(false)
const Regoognition = window.SpeechRecognition || window.webkitSpeechRecognition
const speachRecognition = new Regoognition()
const recordBtn = ref(null)


const toggleMicrophone = () => {
  if (isRecording.value) {
    speachRecognition.stop()
    recordBtn.value.innerText = 'start recording'
  } else {
    speachRecognition.start()
    recordBtn.value.innerText = 'recording...'
  }
}

onMounted(() => {
  speachRecognition.continuous = true
  speachRecognition.interimResults = true

  // Start recording
  speachRecognition.onstart = () => {
    console.log('speach recognition started')
    isRecording.value = true
  }

  // Stop recording
  speachRecognition.onend = () => {
    console.log('speach recognition stopped')
    isRecording.value = false
  }

  // Get transcript
  speachRecognition.onresult = (event) => {
    const transcript = Array.from(event.results)
      .map((result) => result[0])
      .map((result) => result.transcript)
      .join('')
    transcript.value = transcript
  }
})
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600&display=swap');

* {
  font-family: 'Poppins', sans-serif;
}
</style>
