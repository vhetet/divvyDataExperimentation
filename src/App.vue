<template>
  <div id="app">
    <GraphContainer></GraphContainer>
  </div>
</template>

<script>
import GraphContainer from './components/GraphContainer.vue'

export default {
  name: 'App',
  components: {
    GraphContainer
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
  margin-top: 60px;
}
</style>
