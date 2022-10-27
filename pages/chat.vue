<template>
  <div>
    <div class="columns is-mobile">
        <div class="column">
            <input type="text" class="input" v-model="message" @keydown.enter="send">
        </div>
        <div class="column is-1">
            <button class="button is-primary" @click="send">Send</button>
        </div>
    </div>
    <div>
        <div class="columns mb-6" v-for="(message, index) in messages" :key="index" >
            <div class="column" >
                <span v-if="!message.isMe" class="tag is-info is-large" >{{ message.text }}</span>
                <span v-if="message.isMe" class="tag is-primary is-large is-pulled-right" >{{ message.text }}</span>
            </div>  
        </div>
    </div>
  </div>
</template>

<script>
export default {
    mounted(){
        if(process.client){
            this.ws = new WebSocket('ws://localhost:8080');
            this.ws.addEventListener('open', () =>{
                // this.ws.send('hello');
            });
            this.ws.addEventListener('message', (event)=>{               
                this.messages.push({text: event.data, isMe: false});
            });
        }
    },
    data(){
        return {
            ws: null,
            message: '',
            messages: []
        }
    },
    methods: {
        send(){
            this.ws.send(this.message);
            this.messages.push({text: this.message, isMe: true});
            this.message = '';
        }
    }
}
</script>

<style>

</style>