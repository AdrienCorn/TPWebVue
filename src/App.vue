<template>
  <div class="container">
    <div id="result">Currently Saying: </div><br>
<<<<<<< HEAD
    <button id="commencer" v-on:click="commencer"><img id="mic_img" src="./Images/mic_icon.jpg"/></button>
=======
    <button id="commencer" v-on:click="record"><img src="./Images/mic_icon.jpg" height="51" width="51"/></button>
>>>>>>> 8157c5d2dea1348e6eb0582f99e63301d5502987
    <img id="bubble" src="./Images/bubble.jpg" height="28" width="82"/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      isRecord : false,
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
    record() {
      if(!this.isRecord) {
        this.isRecord = !this.isRecord;
        let vm = this;
        this.recognition.continuous = true;
        this.recognition.interimResults = true;
        this.recognition.start();

        this.recognition.onresult = function (event) {
          this.result_transcript = '';
          for (let i = event.resultIndex; i < event.results.length; ++i) {
            this.result_transcript += event.results[i][0].transcript;
          }
          document.getElementById('result').innerHTML = "Currently Saying: " + this.result_transcript;
          for (let j = 0; j < vm.keyWord.length; j++) {
            if (this.result_transcript.indexOf(vm.keyWord[j]) > -1) {
              vm.vibrateSimple();
              vm.arreter();
            }
          }
        };
      }
      else{
        this.isRecord = !this.isRecord;
        this.recognition.stop();
      }
    },
  }
}
</script>

<style>

#commencer {
  order: 2;
  border: 0px;
  background: white;
  center: self;
  height: 30%;
  width: 30%;
  margin-left: auto;
  margin-right: auto;
}

#mic_img {
  height: 100%;
}

#result {
  order: 0;
  center: self;
  margin-left: auto;
  margin-right: auto;
  height: 40px;
  /*margin: auto;*/
  border: 1px solid #000;
  border-radius: 4px;
  font-family: monospace;
  font-size: 18px;
  width: 90%;
}

#bubble {
  order: 1;
  /*center: self;*/
  width: 50%;
  height: 80%;
  margin-left: auto;
  margin-right: auto;
}

.container {
  text-align: center;
  display: grid;
  grid-template-rows: 1fr 1fr 1fr;
  grid-template-columns: 1fr;
}

@media (min-width:720px){
  .container{
    text-align: center;
    display: grid;
    grid-template-rows: 1fr 1fr 1fr;
    grid-template-columns: 1fr;
  }
}
</style>
