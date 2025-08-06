<template>
  <div id="app">
     <canvas-component />
  </div>
</template>

<script>
import CanvasComponent from './components/CanvasComponent.vue';

export default {
  name: 'App',
  components: {
    CanvasComponent
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
