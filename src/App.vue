<template>
  <div id="app">
    <WeatherFeature />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import HelloWorld from './components/HelloWorld.vue';
import '@/styles/index.scss'
import '@mdi/font/css/materialdesignicons.css'
import WeatherFeature from "@/features/WeatherFeature.vue";

@Component({
  components: {
    WeatherFeature,
    HelloWorld,
  },
})
export default class App extends Vue {}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
