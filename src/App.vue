<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <app-map></app-map>
  </div>
</template>

<script>
import Map from './components/leaflet-map.vue';

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  components:{
    'app-map': Map
  }
}
</script>

<style>
body {
  margin: 0;
}
</style>
