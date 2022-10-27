<template>
  <img :src="'https://http.cat/' + error.statusCode">
</template>

<script>
export default {
    props: ['error'],
}
</script>

<style>

</style>