<template>
<div>
    <div class="columns">
        <div class="column is-one-fifth">
            <input class="input" type="number" min="1" max="100" v-model="size" >
        </div>
        <div class="column is-1">
            <input class="input" type="color" v-model="color" >
        </div>
        <div class="column is-2">
            <button class="button" @click="open()">EyeDropper</button>
        </div>
        <div class="column is-1">
            <button class="button" @click="save()">Save</button>
        </div>
        <div class="column is-1">
            <button class="button" @click="Clipboard()">Clipboard</button>
        </div>
    </div>
    <canvas width="800" height="600" ref="mycanvas" @click="click" @mousedown="down" @mouseup="up" @mousemove="move" > </canvas>
</div>
</template>

<script>
export default {
    mounted(){
        let canvas = this.$refs['mycanvas'];
        this.ctx = canvas.getContext('2d');
        this.ctx.fillStyle = 'white';
        this.ctx.fillRect(0, 0, 800, 600);
        // this.ctx.beginPath();
        // this.ctx.arc(100, 100, 40, 0, 2*Math.PI);
        // this.ctx.fillStyle = '#ff0';
        // this.ctx.fill();
    },
    data(){
        return {
            ctx: null,
            isMouseDown: false,
            size: 40,
            color: '#fff'
        }
    },
    methods: {
        click(event){
            this.ctx.beginPath();
            this.ctx.arc(event.offsetX, event.offsetY, this.size, 0, 2*Math.PI);
            this.ctx.fillStyle = this.color;
            this.ctx.fill();
            
        },
        down(event){
            if(event.button == 0){
                this.isMouseDown = true;
            }
        },
        up(event){
            if(event.button == 0){
                this.isMouseDown = false;
            }
        },
        move(event){
            if(this.isMouseDown){
                this.ctx.beginPath();
                this.ctx.arc(event.offsetX, event.offsetY, this.size, 0, 2*Math.PI);
                this.ctx.fillStyle = this.color;
                this.ctx.fill();
            }
        },
        open(){
            if(process.client){
                let eyedropper = new EyeDropper();
                eyedropper.open().then(result => {
                    console.log(result);
                    this.color = result.sRGBHex;
                })
            }
        },
        save(){
            var link = document.createElement('a');
            link.download = 'filename.png';
            link.href = this.$refs['mycanvas'].toDataURL();
            link.click();
        },
        Clipboard(){
            this.$refs['mycanvas'].toBlob(function(blob) { 
                const item = new ClipboardItem({ "image/png": blob });
                navigator.clipboard.write([item]); 
            });
        }
    }
}
</script>

<style scoped>
    canvas {
        border: 3px dashed black;
    }
</style>