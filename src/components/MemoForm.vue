<template>
  <textarea class="memo-input-area" 
    type="text"
    :value="value"
    @input="$emit('input', $event.target.value)"
  >
  </textarea>
</template>

<script>
export default {
  props: ['value']
}
</script>

<style>
.memo-input-area {
  width: 200px;
  height: 300px;
  padding: 8px 10px;
  border: 1px solid skyblue;
}
</style>