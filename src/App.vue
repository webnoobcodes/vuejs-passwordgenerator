<template>
  <div id="app">
    <PasswordGenerator />
  </div>
</template>

<script>
import PasswordGenerator from './components/PasswordGenerator.vue'

export default {
  name: 'app',
  components: {
    PasswordGenerator
  }
}
</script>

<style lang="scss">
  body {
    background-color: #222;
    margin: 0;
    padding: 0;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial;
    text-align: center;
    color: #333;
    font-size: 2rem;
  }
</style>
