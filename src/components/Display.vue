<script setup>
defineProps({
  text: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="display">
    <p>{{ text }}</p>
  </div>
</template>
<style scoped>
  .display {
    font-family: 'Courier New', Courier, monospace;
    width: 100%;
    border: solid white 2px;
    border-radius: 10px;
    min-height: 40px;
  }
  .display p {
    color: lightgreen;
    font-size: 1.5em;
    text-align: right;
    padding: 5px;
    margin: 0;
  }
</style>
