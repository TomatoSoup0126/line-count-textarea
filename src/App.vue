<script setup>
import LineCountTextarea from './components/LineCountTextarea.vue';

const validations = [
  (value) => value === 'apple',
  (value) => value === 'tomato',
]
</script>

<template>
  <div>
    <LineCountTextarea
      :validations="validations"
    />
  </div>
</template>

<style scoped>
</style>
