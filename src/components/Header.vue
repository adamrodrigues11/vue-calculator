<script setup>
defineProps({
  title: {
    type: String,
    required: true
  }
})
</script>

<template>
  <header>
    <h1>{{ title }}</h1>
  </header>
</template>

<style scoped>
  h1 {
      font-size: 1.2em;
      padding: 10px;
      margin: 0;
  }
</style>
