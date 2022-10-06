<template>
  <div class="w-3/5 m-auto h-screen flex items-center justify-center flex-col">
    <button class="btn btn-success" @click="ToggleMic" ref="recordBtn">start recording</button>

    <!-- Transcript -->
    <div class="transcript my-4 font-bold text-justify" v-text="transcript"></div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const transcript = ref('')
const isRecording = ref(false)
const Regoognition = window.SpeechRecognition || window.webkitSpeechRecognition
const sr = new Regoognition()
const recordBtn = ref(null)


onMounted(() => {
  sr.continuous = true
  sr.interimResults = true
  sr.onstart = () => {
    console.log('SR Started')
    isRecording.value = true
  }
  sr.onend = () => {
    console.log('SR Stopped')
    isRecording.value = false
  }
  sr.onresult = (evt) => {
    for (let i = 0; i < evt.results.length; i++) {
      const result = evt.results[i]
      if (result.isFinal) CheckForCommand(result)
    }
    const t = Array.from(evt.results)
      .map(result => result[0])
      .map(result => result.transcript)
      .join('')

    transcript.value = t
  }
})
const CheckForCommand = (result) => {
  const t = result[0].transcript;
  if (t.includes('stop recording')) {
    sr.stop()
  } else if (
    t.includes('what is the time') ||
    t.includes('what\'s the time')
  ) {
    sr.stop()
    alert(new Date().toLocaleTimeString())
    setTimeout(() => sr.start(), 100)
  }
}
const ToggleMic = () => {
  if (isRecording.value) {
    sr.stop()
  } else {
    sr.start()
  }
}

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600&display=swap');

* {
  font-family: 'Poppins', sans-serif;
}
</style>
