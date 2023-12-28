<script setup>
import { ref, computed, watch, nextTick } from 'vue'
const props = defineProps({
  lineNumber: {
    type: Number,
    default: 0
  },
  lineText: {
    type: String,
    default: ''
  },
  isError: {
    type: Boolean,
    default: false
  },
  lineWidth: {
    type: Number,
    default: 0
  }
})

const lineText = ref(null)
const lineTextWidth = ref(0)

const columnCount = computed(() => {
  return Math.ceil(lineTextWidth.value / props.lineWidth) || 1
})

const updateLineTextWidth = () => {
  lineTextWidth.value = lineText?.value?.clientWidth || 0
}

watch(() => props.lineText, () => {
  nextTick(() => {
    updateLineTextWidth()
  })
})
</script>

<template>
<span
  class="line-number"
  :class="{
    'line-number--error': props.isError,
  }"
  :style="[
    `--column-count: ${columnCount}`,
] "
>
  {{ props.lineNumber }}
</span>
<span
  ref="lineText"
  class="line-text"
>
  {{ props.lineText }}
</span>
</template>

<style scoped>
.line-number {
  display: block;
  font-size: 14px;
  height: calc(21px * var(--column-count));
}
.line-number--error {
  color: red;
}

.line-text {
  position: absolute;
  top: -9999px;
  visibility: hidden;
  font-size: 14px;
}
</style>