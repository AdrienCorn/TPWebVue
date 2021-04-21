<template>
  <div id="result">Currently Saying: </div><br>
  <button id="commencer" v-on:click="commencer"><img src="./Images/mic_icon.jpg" height="512" width="512"/></button>
  <button id="arreter" v-on:click="arreter">arreter</button>
  <img id="bubble" src="./Images/bubble.jpg" height="282" width="820"/>
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
  grid-column-start: 2;
}

#result {
  grid-column-start: 1;
  border: 1px solid #000;
  border-radius: 4px;
  font-family: monospace;
  font-size: 18px;
  width: 800px;
  margin: auto;
}

#bubble {
  grid-column-start: 3;
}

body {
  text-align: center;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 1fr;

}
</style>
