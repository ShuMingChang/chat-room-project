<template>
  <div id="app">
    <chatRoom></chatRoom>
  </div>
</template>

<script>
import chatRoom from './components/chatRoom'

export default {
  name: 'App',
  components: {
    chatRoom
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
