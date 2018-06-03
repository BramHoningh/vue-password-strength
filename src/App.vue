<template>
  <div id="app">
    <DHKPasswordInput />
  </div>
</template>

<script>
import DHKPasswordInput from './components/DHKPasswordInput'

export default {
  name: 'app',
  components: {
    DHKPasswordInput
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
