<template>
  <button class="button is-primary" @click="copy()" >Copy</button>
</template>

<script>
export default {
    methods: {
        copy(){
            navigator.clipboard.writeText("<empty clipboard>").then(() => {

            });
        }
    }
}
</script>

<style>

</style>