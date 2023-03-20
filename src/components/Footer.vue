<script setup>
import { getYear } from "../utilities/utils";
defineProps({
  author: {
    type: String,
    required: true
  }
})

</script>

<template>
  <footer>
    <p>&copy; {{ getYear() + " " + author }}</p>
  </footer>
</template>

<style scoped>
    footer {
        text-align: center;
        padding: 10px;
    }
</style>