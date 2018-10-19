<template>
  <div id="app">
    <top-bar></top-bar>
    <router-view></router-view>
  </div>
</template>

<script>
import TopBar from '@/components/LandingPage/TopBar'

  export default {
    name: 'ledpro',
    components: {
      TopBar
    },
  }
</script>

<style>
  /* CSS */
</style>
