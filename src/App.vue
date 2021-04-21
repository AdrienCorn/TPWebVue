<template>
  <div id="interim">Currently Saying: </div><br>
  <div id="results">Press Start!</div>

  <button id="commencer" v-on:click="commencer">commencer</button>
  <button id="arreter" v-on:click="arreter">arreter</button>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      recognition : new window.webkitSpeechRecognition(),
      final_transcript : '',
      interim_transcript : '',
    }
  },
  methods: {
    commencer() {
      this.recognition.continuous = true;
      this.recognition.interimResults = true;
      this.recognition.start();

      this.recognition.onresult = function(event) {
        this.interim_transcript = '';

        for (let i = event.resultIndex; i < event.results.length; ++i) {
          if (event.results[i].isFinal) {
            this.final_transcript += event.results[i][0].transcript;
          } else {
            this.interim_transcript += event.results[i][0].transcript;
          }
        }
        document.getElementById('results').innerHTML = this.final_transcript;
        document.getElementById('interim').innerHTML = "Currently Saying: " + this.interim_transcript;
      };
    },
    arreter() {
      this.recognition.stop();
    },
  }
}
</script>

<style>
#results {
  border: 1px solid #000;
  width: 800px;
  height: 200px;
  margin: auto;
  border-radius: 4px;
  font-family: monospace;
  font-size: 18px;
}

#interim {
  border: 1px solid #000;
  border-radius: 4px;
  font-family: monospace;
  font-size: 18px;
  width: 800px;
  margin: auto;
}

body {
  text-align: center;
}
</style>
