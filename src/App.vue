<template>
  <div id="app">
  <Header/>
  <MemoApp/>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import MemoApp from './components/MemoApp.vue'

export default {
  name: 'app',
  components: {
    Header,
    MemoApp,
  }
}
</script>

<style>
</style>
