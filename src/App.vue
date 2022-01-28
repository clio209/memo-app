<template>
<div id="app">
<Header/>
<MemoApp/>
<!-- <Memo/> -->
 </div>
</template>

<script>
import Header from './components/Header.vue'
import MemoApp from './components/MemoApp.vue'
// import Memo from './components/Memo.vue'


export default {
  name: 'app',
  components: {
    Header,
    MemoApp,
    // Memo
  }
}
</script>

<style>

</style>
