<template>
  <div class="w-3/5 m-auto h-screen flex items-center justify-center flex-col">
    <button class="btn btn-success" @click="ToggleMic" ref="recordBtn">start recording</button>

    <!-- Transcript -->
    <div class="transcript my-4 font-bold text-justify" v-text="transcript"></div>

    <!-- Modal -->
    <!-- The button to open modal -->
    <label for="my-modal" class="btn modal-button hidden" ref="modalBtn">open modal</label>

    <!-- Put this part before </body> tag -->
    <input type="checkbox" id="my-modal" class="modal-toggle" />
    <div class="modal modal-bottom sm:modal-middle">
      <div class="modal-box">
        <h3 class="font-bold text-lg">Speach Recognition</h3>
        <p class="py-4">{{ message }}</p>
        <div class="modal-action">
          <label for="my-modal" class="btn">Yay!</label>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const message = ref('')
const transcript = ref('')
const isRecording = ref(false)
const Regoognition = window.SpeechRecognition || window.webkitSpeechRecognition
const sr = new Regoognition()
const recordBtn = ref(null)
const modalBtn = ref(null)


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
    t.includes('what is the time') || t.includes('what\'s the time')) {
    sr.stop()
    message.value = 'The time is ' + new Date().toLocaleTimeString()
    modalBtn.value.click()
    setTimeout(() => sr.start(), 100)
  } else if (t.includes('what is today\'s date') 
        || t.includes('what\'s today\'s date') 
        || t.includes('what is the date)')
        || t.includes('what\'s the date')) {
    sr.stop()
    message.value = 'Today is ' + new Date().toLocaleDateString()
    modalBtn.value.click()
    setTimeout(() => sr.start(), 100)
  }
}
const ToggleMic = () => {
  if (isRecording.value) {
    sr.stop()
    recordBtn.value.innerText = 'start recording'
    recordBtn.value.classList.replace('btn-error', 'btn-success')
  } else {
    sr.start()
    recordBtn.value.innerText = 'stop recording'
    recordBtn.value.classList.replace('btn-success', 'btn-error')
  }
}

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600&display=swap');

* {
  font-family: 'Poppins', sans-serif;
}
</style>
