<template>
  <div id="app">
    <img alt="beer icon" src="./assets/logo.png" class="logo">
    <h1>Let's Buy Some Beer</h1>
    <div class="home">
      <BeerUnitPrice/>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import BeerUnitPrice from './components/BeerUnitPrice.vue';

@Component({
  components: {
    BeerUnitPrice,
  },
})
export default class App extends Vue {}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 38px;
}
img.logo {
  max-width: 100px;
}
</style>
