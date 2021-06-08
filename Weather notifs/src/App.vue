<template>
  <div id="app">
    <div class="container">
      <WeatherData stationID="93000" APIKey="vcj1ubsnlfuykft25arnqmnjz3sgdduo"
      APISecret="bi3pjtjxhgrqnjci6h1zjmdr9xjs59vm"/>
    </div>
  </div>
</template>

<script>
import WeatherData from './components/WeatherData.vue';

export default {
  name: 'App',
  components: {
    WeatherData,
  },
};
</script>

<style>

#app {
  font-family:sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 50%;
  height:20%;
  margin: auto
}

.container{
  border-style: solid;
  width:100%;
}
</style>
