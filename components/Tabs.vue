<template>
  <div class="tabs">
    <ul>
      <li :class="{'is-active':current==link.path}" v-for="(link,index) in links" :key="index">
        <nuxt-link :to="link.path" @click="active=index">{{ link.name }}</nuxt-link>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ['links', 'current']
  
}
</script>

<style>

</style>