<template>
  <div id="app">
    <Loader />
    <Hello :login = "login" />
    <Form @loaded="updateStatus"/>
  </div>
</template>

<script>
import Hello from './components/Hello.vue'
import Form from './components/Form.vue'
import Loader from './components/Loader.vue'

export default {
  name: 'app',
  components: {
    Hello,
    Loader,
    Form
  },
  data: () => {
    return {
      login: false,
    }
  },
  methods: {
    updateStatus() {
      this.login = true;
    } 
  }
}
</script>

<style lang="scss">
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2e2e2e;
  margin: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
}

html, body {
  width: 100%;
  height: 100%;
  background: linear-gradient(to bottom, gray, #ffffff);
}

body {
  margin: 0;
}

</style>
