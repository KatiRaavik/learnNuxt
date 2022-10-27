<template>
  <div>
    <input class="input" type="text" v-model="message">
    <button class="button is-primary" @click="speak()">speak</button>
    <div class="select">
        <select v-model="selectedVoice">
            <option v-for="(voice,index) in voices" :key="index" :value="index">{{voice.name}}</option>
        </select>
    </div>
  </div>
</template>

<script>
import annyang from 'annyang';
export default {
    mounted(){        
        if(process.client){
            if (annyang) {
                console.log(annyang);
                // Let's define a command.
                var commands = {
                    'hello': function() { 
                        console.log('hello');
                        alert('Hello world!'); 
                    },
                    'cool beans': () => {
                        console.log('cool beans');                        
                    }
                };  
                // Add our commands to annyang
                annyang.addCommands(commands);
                // Start listening.
                annyang.start();
                }
            let interval = setInterval( () => {
                this.voices = speechSynthesis.getVoices();
                if(this.voices.length >0){
                    clearInterval(interval);
                }
            }, 100);
        }        
    },
    data(){
        return {
            message: '',
            voices: [],
            selectedVoice: 0
        }
    },
    methods: {
        speak(){
            let utterance = new SpeechSynthesisUtterance(this.message);
            utterance.voice = this.voices[this.selectedVoice];
            speechSynthesis.speak(utterance);
        }
    }
}
</script>

<style>

</style>