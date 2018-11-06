<template>
  <div id="app">
    <VuePasswordStrength />
  </div>
</template>

<script>
import VuePasswordStrength from './components/VuePasswordStrength'

export default {
  name: 'app',
  components: {
      VuePasswordStrength
  },
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  width: 300px;
}
</style>
