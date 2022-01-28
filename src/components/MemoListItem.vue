<template>
  <li>
    <a href="#" @click="increment">{{ todo.title }}</a>
  </li>
</template>

<script>
export default {
props: {
  todo: {
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