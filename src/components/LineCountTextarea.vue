<script setup>
import { ref, computed, defineProps, onMounted, watch, nextTick } from 'vue'
import LineText from './LineText.vue'

const props = defineProps({
  validations: {
    type: Array,
    default: () => ([])
  },
  backgroundColor: {
    type: String,
    default: '#F9FBFD'
  },
  fontColor: {
    type: String,
    default: 'black'
  },
  borderRadius: {
    type: String,
    default: '4px'
  },
  borderColor: {
    type: String,
    default: '#B4BAC5'
  },
  outlineColor: {
    type: String,
    default: '#408FFF40'
  },
  focusColor: {
    type: String,
    default: '#408FFF'
  }
})

const inputTextarea = ref(null)
const inputTextareaWidth = ref(0)
const inputTextareaHeight = ref(0)

const input = ref('')
const isFocus = ref(false)

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

const getInputTextareaWidth = () => {
  inputTextareaWidth.value = inputTextarea.value.offsetWidth
}

const getInputTextareaHeight = () => {
  inputTextareaHeight.value = inputTextarea.value.scrollHeight
}

watch(() => input.value, () => {
  nextTick(() => {
    getInputTextareaHeight()
  })
})

onMounted(() => {
  getInputTextareaWidth()
  getInputTextareaHeight()
})

</script>

<template>
  <div>
    <div
    class="textarea-container"
    :class="{
      'textarea-container--focus': isFocus,
      'textarea-container--error': errorLines.length > 0,
    }"
    :style="[
      `--background-color: ${props.backgroundColor}`,
      `--font-color: ${props.fontColor}`,
      `--border-radius: ${props.borderRadius}`,
      `--focus-color: ${props.focusColor}`,
      `--outline-color: ${props.outlineColor}`,
      `--border-color: ${props.borderColor}`,
      `--line-height: ${props.lineHeight}`,
    ]"
  >
    <div class="line-container">
      <LineText
        v-for="lineNumber in linesCount"
        :key="lineNumber"
        :line-number="lineNumber"
        :line-text="lines[lineNumber - 1]"
        :is-error="errorLines.includes(lineNumber)"
        :line-height="props.lineHeight"
      />
    </div>
    <textarea
      class="input-textarea"
      ref="inputTextarea"
      v-model="input"
      :style="[
        `height: ${inputTextareaHeight || 200}px;`,
      ]"
      @focus="isFocus = true"
      @blur="isFocus = false"
    />
    </div>
    <div>
      <slot name="error" :errorLines="errorLines" />
    </div>
  </div>
</template>

<style scoped>
.textarea-container {
  display: inline-flex;
  gap: 10px;
  font-family: monospace;
  padding: 20px 10px;
  overflow: auto;
  height: 200px;
  line-height: 21px;
  color: var(--font-color);
  background-color: var(--background-color);
  border-radius: var(--border-radius);
  border: 1px solid var(--border-color);
  transition: all 0.05s linear;
}
.textarea-container--focus {
  border: 1px solid var(--focus-color);
  outline: 4px solid var(--outline-color);
}
.textarea-container--error {
  border: 1px solid red;
  outline: none;
}
.input-textarea {
  font-size: 14px;
  overflow-y: hidden;
  padding: 0;
  border: 0;
  line-height: 21px;
  color: var(--font-color);
  background-color: var(--background-color);
  min-width: 500px;
  outline: none;
  resize: none;
}
</style>
