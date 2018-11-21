<template>
  <div id="app">
    <bankView/>
  </div>
</template>

<script>
import bankView from './components/bankView'
import 'vue-material/dist/vue-material.min.css'
import "./styles/styles.scss";
export default {
  name: 'App',
  components: {
    bankView
  }
}
</script>

<style>
#app{
  width:90%;
  margin: 0 auto;
  height:100%;
}
body{
  background: #000;
  font-family: 'Open Sans', sans-serif;
  
}
</style>
