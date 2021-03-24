<template>
  <v-app>
    <Header/>
    <h1>Control transfer</h1>
    
    <Form/>
  </v-app>
</template>

<script>
import Form from "./components/Form"
import Header from "./components/Header"
export default {
  name: 'App',
  components: {
    Form, Header
  },
  data: () => ({
      //
  }),
};

  
</script>
