<script setup>
import { ref, computed, defineProps } from 'vue'

const props = defineProps({
  validations: {
    type: Array,
    default: () => ([])
  }
})

const input = ref('')

const lines = computed(() => {
  return input.value.split('\n')
})

const linesCount = computed(() => {
  return lines.value.length
})

const errorLines = computed(() => {
  const invalidLines = []
  lines.value.forEach((line, index) => {
    const isInvalid = props.validations.some((validation) => {
      return validation(line)
    })
    if (isInvalid) {
      invalidLines.push(index + 1)
    }
  })
  return invalidLines
})

</script>

<template>
  <div class="textarea-container">
    <div class="line-container">
      <span
        v-for="line in linesCount"
        :key="line"
        class="line-number"
        :class="{
          'line-number--error': errorLines.includes(line)
        }"
      >
      </span>
    </div>
    <textarea
      class="input-textarea"
      v-model="input"
      rows="10"
      cols="30"
    />
  </div>
  <div>Error Lines: {{ errorLines }}</div>
</template>

<style scoped>
.textarea-container {
  display: inline-flex;
  gap: 10px;
  font-family: monospace;
  line-height: 21px;
  background: #282a3a;
  border-radius: 2px;
  padding: 20px 10px;

  height: 200px;
  overflow: auto;
}
.line-container {
  width: 20px;
  text-align: right;
  height: 9999px;
  .line-number {
    display: block;
    counter-increment: linenumber;
  }
  .line-number--error {
    color: #ff0000;
  }
  .line-number::before {
    content: counter(linenumber);
    display: block;
    color: #506882;
  }
  .line-number--error::before {
    color: #ff0000;
  }
}
.input-textarea {
  height: 9999px;
  line-height: 21px;
  overflow-y: hidden;
  padding: 0;
  border: 0;
  background: #282a3a;
  color: #FFF;
  min-width: 500px;
  outline: none;
  resize: none;
}
</style>
