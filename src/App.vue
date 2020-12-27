<template>
  <div id="app">
    <Header/>
    <h1>Control transfer</h1>
    
    <Form/>
    
  </div>
</template>

<script>
import Form from "./components/Form"
import Header from "./components/Header"

export default {
  name: 'App',
  components: {
    Form, Header
  }
}
</script>

<style>
body {
  margin: 0; padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50; 
}
</style>
