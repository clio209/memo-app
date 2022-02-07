<template>
  <form>
    <textarea type="text" @input="inputEvent">
    </textarea>
  </form>
</template>

<script>
export default {
  props: {
    text: {
      type:String
    },
  },
  methods: {
    inputEvent: function () {
      this.$emit('inputEvent')
    }
  }
  }

</script>

<style>
</style>
