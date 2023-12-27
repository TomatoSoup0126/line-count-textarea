<script setup>
import LineCountTextarea from './components/LineCountTextarea.vue';

const validations = [
  (value) => value === 'apple',
  (value) => value === 'tomato',
]

const ipv4Validations = [
  (value) => {
    if (value === '') {
      return false
    }
    const ipv4Regex = /^(?:[0-9]{1,3}\.){3}[0-9]{1,3}$/
    return !ipv4Regex.test(value)
  }
]
</script>

<template>
  <div>
    <LineCountTextarea
      :validations="ipv4Validations"
    >
      <template #error="{ errorLines }">
        <div class="mt-4">
          {{ errorLines.length > 0 ? `列 ${errorLines.join(', ')}：IP 位址格式不正確，請使用 IPv4 格式`: '' }}
        </div>
      </template>
    </LineCountTextarea>

    <LineCountTextarea
      class="mt-4"
      :validations="validations"
      background-color="#f0f0f0"
      font-color="#000000"
      border-color="black"
      border-radius="10px"
      outline-color="blue"
      focus-color="yellow"
      line-height="25px"
    >
      <template #error="{ errorLines }">
        <div 
          v-if="errorLines.length > 0"
          class="mt-4"
        >
          {{ `列 ${errorLines.join(', ')}： 不能寫 tomato 或 apple` }}
        </div>
      </template>
    </LineCountTextarea>
  </div>
</template>

<style scoped>
.mt-4 {
  margin-top: 1rem;
}
</style>
