<template>
  <div id="app">
    <Header/>
    <main>
      <AddressDiscover/>
    </main>
  </div>
</template>

<script>
import AddressDiscover from './components/AddressDiscover'
import Header from './components/Header'
import Vue from 'vue'

import BootstrapVue from 'bootstrap-vue'
import 'bootstrap/dist/css/bootstrap-grid.css'

Vue.use(BootstrapVue);

export default {
  name: 'App',
  components: {
    AddressDiscover,
    Header,
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Lato');
body {
  margin: 0;
  padding: 0;
  background-color: #403F4C;
  font-family: 'Lato', sans-serif;
}
* {
  box-sizing: border-box;

}
main {
  margin: 40px 0;
}
</style>
