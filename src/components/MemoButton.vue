<template>
  <button
    class="MyButton"
    @click="buttonClick"
  ><slot></slot></button>
</template>

<script>
export default {
  props: {
    text: {
      type:String
    },
  },
  methods: {
    buttonClick: function() {
      this.$emit('buttonClick')
    }
  }
}
</script>

<style>
.MyButton {
  border-radius: 1.5em;
  box-shadow: 0 0.2em 0.5em rgba(0, 0, 0, 0.2);
  padding: 1em 2em;
  font-weight: bold;
  text-decoration: none;
}
.MyButton.left_arrow {
  padding-left: 0;
}
.MyButton.left_arrow::before {
  content: "〈";
  padding-right: 1em;
}
.MyButton.right_arrow {
  padding-right: 0;
}
.MyButton.right_arrow::after {
  padding-left: 1em;
  content: "〉";
}
</style>