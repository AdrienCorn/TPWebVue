<template>
  <div class="container">
    <div id="result">Currently Saying: </div><br>
    <button id="commencer" v-on:click="commencer"><img src="./Images/mic_icon.jpg" height="51" width="51"/></button>
    <img id="bubble" src="./Images/bubble.jpg" height="28" width="82"/>
  </div>
  <button id="arreter" v-on:click="arreter">arreter</button>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      recognition : new window.webkitSpeechRecognition(),
      result_transcript : '',
      keyWord : ["vibre","tremble","vibration"],
    }
  },
  methods: {
    vibrateSimple() {
      navigator.vibrate(200);
      console.log("vibre");
    },
    commencer() {
      let vm = this;
      this.recognition.continuous = true;
      this.recognition.interimResults = true;
      this.recognition.start();

      this.recognition.onresult = function(event) {
        this.result_transcript = '';
        for (let i = event.resultIndex; i < event.results.length; ++i) {
          this.result_transcript += event.results[i][0].transcript;
        }
        document.getElementById('result').innerHTML = "Currently Saying: " + this.result_transcript;
        for (let j = 0; j < vm.keyWord.length; j ++) {
          if (this.result_transcript.indexOf(vm.keyWord[j]) > -1) {
            vm.vibrateSimple();
            vm.arreter();
          }
        }
      };
    },
    arreter() {
      this.recognition.stop();
    },

  }
}
</script>

<style>

#commencer {
  order: 2;
  center: self;
  margin-left: auto;
  margin-right: auto;
}

#result {
  order: 0;
  center: self;
  margin-left: auto;
  margin-right: auto;
  border: 1px solid #000;
  border-radius: 4px;
  font-family: monospace;
  font-size: 18px;
  width: 800px;
  margin: auto;
}

#bubble {
  order: 1;
  center: self;
  margin-left: auto;
  margin-right: auto;
}

.container {
  text-align: center;
  display: grid;
  grid-template-rows: 1fr 1fr 1fr;
  grid-template-columns: 1fr;
}
</style>
