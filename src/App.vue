<template>
  <div id="app">
    <TableWrapper/>
  </div>
</template>

<script>
import TableWrapper from './components/TableWrapper.vue'

export default {
  name: 'App',
  components: {
    TableWrapper
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
