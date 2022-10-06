<template>
  <div class="w-3/5 m-auto h-screen flex items-center justify-center flex-col">
    <button class="btn btn-success" @click="ToggleMic" ref="recordBtn">
      <div class="recording-circle" v-if="isRecording"></div>
      <div ref="recordBtnText">start recording</div>
    </button>

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
import { onMounted, ref } from "vue";

const message = ref("");
const transcript = ref("");
const isRecording = ref(false);
const Regoognition = window.SpeechRecognition || window.webkitSpeechRecognition;
const sr = new Regoognition();
const recordBtn = ref(null);
const recordBtnText = ref(null);
const modalBtn = ref(null);

onMounted(() => {
  sr.continuous = true;
  sr.interimResults = true;
  sr.onstart = () => {
    console.log("SR Started");
    isRecording.value = true;
  };
  sr.onend = () => {
    console.log("SR Stopped");
    isRecording.value = false;
  };
  sr.onresult = (evt) => {
    for (let i = 0; i < evt.results.length; i++) {
      const result = evt.results[i];
      if (result.isFinal) CheckForCommand(result);
    }
    const t = Array.from(evt.results)
      .map((result) => result[0])
      .map((result) => result.transcript)
      .join("");

    transcript.value = t;
  };
});
const CheckForCommand = (result) => {
  const t = result[0].transcript;
  if (t.includes("stop recording")) {
    sr.stop();
  } else if (t.includes("what is the time") || t.includes("what's the time")) {
    sr.stop();
    message.value = "The time is " + new Date().toLocaleTimeString();
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (
    t.includes("what is today's date") ||
    t.includes("what's today's date") ||
    t.includes("what is the date)") ||
    t.includes("what's the date")
  ) {
    sr.stop();
    message.value = "Today is " + new Date().toLocaleDateString();
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("open google")) {
    sr.stop();
    window.open("https://google.com", "_blank");
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("open youtube")) {
    sr.stop();
    window.open("https://youtube.com", "_blank");
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("open facebook")) {
    sr.stop();
    window.open("https://facebook.com", "_blank");
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("open twitter")) {
    sr.stop();
    window.open("https://twitter.com", "_blank");
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("open instagram")) {
    sr.stop();
    window.open("https://instagram.com", "_blank");
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("open github")) {
    sr.stop();
    window.open("https://github.com", "_blank");
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("open stackoverflow")) {
    sr.stop();
    window.open("https://stackoverflow.com", "_blank");
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("open codepen")) {
    sr.stop();
    window.open("https://codepen.io", "_blank");
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("how are you")) {
    sr.stop();
    message.value = "I am fine, thank you";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your name")) {
    sr.stop();
    message.value = "My name is Speach Recognition";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your age")) {
    sr.stop();
    message.value = "I am 1 day old";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite color")) {
    sr.stop();
    message.value = "My favorite color is blue";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite food")) {
    sr.stop();
    message.value = "My favorite food is pizza";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite movie")) {
    sr.stop();
    message.value = "My favorite movie is Avengers";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite song")) {
    sr.stop();
    message.value = "My favorite song is Shape of you";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite book")) {
    sr.stop();
    message.value = "My favorite book is Harry Potter";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite game")) {
    sr.stop();
    message.value = "My favorite game is PUBG";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite sport")) {
    sr.stop();
    message.value = "My favorite sport is Cricket";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite country")) {
    sr.stop();
    message.value = "My favorite country is India";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite food")) {
    sr.stop();
    message.value = "My favorite food is pizza";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite city")) {
    sr.stop();
    message.value = "My favorite city is Mumbai";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite state")) {
    sr.stop();
    message.value = "My favorite state is Maharashtra";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite place")) {
    sr.stop();
    message.value = "My favorite place is Goa";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite animal")) {
    sr.stop();
    message.value = "My favorite animal is Dog";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite bird")) {
    sr.stop();
    message.value = "My favorite bird is Peacock";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite flower")) {
    sr.stop();
    message.value = "My favorite flower is Rose";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite fruit")) {
    sr.stop();
    message.value = "My favorite fruit is Apple";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite vegetable")) {
    sr.stop();
    message.value = "My favorite vegetable is Potato";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite car")) {
    sr.stop();
    message.value = "My favorite car is BMW";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  } else if (t.includes("what is your favorite bike")) {
    sr.stop();
    message.value = "My favorite bike is Harley Davidson";
    modalBtn.value.click();
    setTimeout(() => sr.start(), 100);
  }
};
const ToggleMic = () => {
  if (isRecording.value) {
    sr.stop();
    recordBtnText.value.innerText = "start recording";
    recordBtn.value.classList.replace("btn-error", "btn-success");
  } else {
    sr.start();
    recordBtnText.value.innerText = "stop recording";
    recordBtn.value.classList.replace("btn-success", "btn-error");
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600&display=swap");

* {
  font-family: "Poppins", sans-serif;
}

.recording-circle {
  background-color: red;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  animation: ease pulse 2s infinite;
  margin-right: 10px;
  outline: 1px solid #fff;
}

@keyframes pulse {
  0% {
    background-color: red;
  }

  50% {
    background-color: #f06c6c;
  }

  100% {
    background-color: red;
  }
}
</style>
