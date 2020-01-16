<template>
  <div id="app">
    <weather-map></weather-map>
  </div>
</template>
<script>
import WeatherMap from '@/components/WeatherMap'

export default {
  name: 'app',
  components: {
    WeatherMap
  }
}
</script>
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'montserrat', sans-serif;
}
</style>