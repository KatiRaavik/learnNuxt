<template>
  <button class="button is-primary" @click="open()" :style="bgcolor">eyedropper</button>
</template>

<script>
export default {
    methods: {
        open(){
            if(process.client){
                let eyedropper = new EyeDropper();
                eyedropper.open().then(result => {
                    console.log(result);
                    this.color = result.sRGBHex;
                });
            
            }
            
        }
    },
    data(){
        return {
            color: null
        }
    },
    computed: {
        bgcolor(){
            if(this.color){
                return 'background:' + this.color;
            }
        return '';
        }
    }
}
</script>

<style>

</style>