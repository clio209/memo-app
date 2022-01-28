<template>
  <li>
    <a href="#" @click="increment">{{ memo.title }}</a>
  </li>
</template>

<script>
export default {
props: {
  memo: {
    type:Object
  },
},
methods: {
  increment: function() {
    this.$emit('increment')
  }
}
}
</script>

<style>
</style>