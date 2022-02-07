<template>
  <li>
    <a href="#" @click="titleClick">{{ memo.title }}</a>
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
  titleClick: function() {
    this.$emit('titleClick')
  }
}
}
</script>

<style>
</style>
