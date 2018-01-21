<template>
  <div id="app">
    <Header/>
    <AddressDiscover/>
  </div>
</template>

<script>
import AddressDiscover from './components/AddressDiscover'
import Header from './components/Header'
import Vue from 'vue'

import BootstrapVue from 'bootstrap-vue'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

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
#app {
}
</style>
