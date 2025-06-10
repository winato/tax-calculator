<template>
  <div class="container">
    <h1>Calculator RSU (Netherlands)</h1>

    <div class="input-group">
      <label>Amount RSU:</label>
      <input type="number" v-model.number="rsuCount" min="0" />
    </div>

    <div class="input-group">
      <label>Price for one RSU (€):</label>
      <input type="number" v-model.number="rsuPrice" min="0" />
    </div>

    <div class="input-group checkbox">
      <input type="checkbox" id="ruling" v-model="hasRuling" />
      <label for="ruling">I have 30% ruling</label>
    </div>

    <hr />

    <div class="result">
      <p><strong>Gross total:</strong> €{{ grossAmount.toFixed(2) }}</p>
      <p><strong>Taxable amount:</strong> €{{ taxableAmount.toFixed(2) }}</p>
      <p><strong>Tax (~39.21%):</strong> €{{ taxAmount.toFixed(2) }}</p>
      <p><strong>Nett income:</strong> €{{ netAmount.toFixed(2) }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const rsuCount = ref(0)
const rsuPrice = ref(0)
const hasRuling = ref(false)

const grossAmount = computed(() => rsuCount.value * rsuPrice.value)

const taxableAmount = computed(() => {
  return hasRuling.value ? grossAmount.value * 0.7 : grossAmount.value
})

const taxRate = 0.3921

const taxAmount = computed(() => taxableAmount.value * taxRate)
const netAmount = computed(() => grossAmount.value - taxAmount.value)
</script>

<style scoped>
.container {
  max-width: 500px;
  margin: auto;
  padding: 2rem;
  font-family: Arial, sans-serif;
}
.input-group {
  margin-bottom: 1rem;
}
input[type="number"] {
  width: 100%;
  padding: 0.5rem;
}
.checkbox {
  display: flex;
  align-items: center;
}
.result p {
  font-size: 1.1rem;
}
</style>
