<template>
  <section class="calculator-card">
    <header class="hero-panel">
      <div>
        <p class="eyebrow">RSU calculator</p>
        <h1>Estimate your Dutch RSU net payout</h1>
        <p class="hero-copy">
          Enter the number of RSUs and price per share to see estimated gross, taxable amount, withholding tax,
          and net income with or without the 30% ruling.
        </p>
      </div>
      <div class="hero-badge">
        <span>Tax rate</span>
        <strong>39.21%</strong>
      </div>
    </header>

    <div class="layout-grid">
      <div class="input-panel">
        <div class="field-row">
          <label for="rsu-count">Amount of RSUs</label>
          <input
            id="rsu-count"
            type="number"
            min="0"
            inputmode="numeric"
            v-model.number="rsuCount"
            placeholder="0"
          />
        </div>

        <div class="field-row">
          <label for="rsu-price">Price per RSU (€)</label>
          <input
            id="rsu-price"
            type="number"
            min="0"
            inputmode="decimal"
            step="0.01"
            v-model.number="rsuPrice"
            placeholder="0.00"
          />
        </div>

        <div class="toggle-row">
          <div class="toggle-label">
            <span>I have 30% ruling</span>
            <small>Taxable income reduced by 30%</small>
          </div>
          <label class="switch">
            <input type="checkbox" v-model="hasRuling" />
            <span class="slider" />
          </label>
        </div>
      </div>

      <aside class="result-panel">
        <div class="result-header">
          <p class="result-label">Your RSU payout breakdown</p>
          <span class="result-status">{{ hasRuling ? 'With 30% ruling' : 'Without ruling' }}</span>
        </div>

        <div class="result-list">
          <div class="result-item">
            <span>Gross total</span>
            <strong>{{ formatCurrency(grossAmount) }}</strong>
          </div>
          <div class="result-item">
            <span>Taxable amount</span>
            <strong>{{ formatCurrency(taxableAmount) }}</strong>
          </div>
          <div class="result-item">
            <span>Tax due</span>
            <strong>{{ formatCurrency(taxAmount) }}</strong>
          </div>
          <div class="result-item highlight">
            <span>Estimated net income</span>
            <strong>{{ formatCurrency(netAmount) }}</strong>
          </div>
        </div>
      </aside>
    </div>
  </section>
</template>

<script setup>
import { computed, ref } from 'vue'

const rsuCount = ref(0)
const rsuPrice = ref(0)
const hasRuling = ref(false)

const grossAmount = computed(() => rsuCount.value * rsuPrice.value)
const taxableAmount = computed(() => (hasRuling.value ? grossAmount.value * 0.7 : grossAmount.value))

const taxRate = 0.3921
const taxAmount = computed(() => taxableAmount.value * taxRate)
const netAmount = computed(() => grossAmount.value - taxAmount.value)

const currencyFormatter = new Intl.NumberFormat('en-US', {
  style: 'currency',
  currency: 'EUR',
  minimumFractionDigits: 2,
  maximumFractionDigits: 2,
})

const formatCurrency = (value) => currencyFormatter.format(value)
</script>

<style scoped>
.calculator-card {
  max-width: 760px;
  margin: 2rem auto;
  padding: 1.75rem;
  border-radius: 30px;
  background: radial-gradient(circle at top left, rgba(58, 123, 213, 0.16), transparent 35%),
    linear-gradient(180deg, #ffffff 0%, #f5f8fb 100%);
  box-shadow: 0 32px 80px rgba(23, 43, 77, 0.12);
  font-family: Inter, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  color: #102a43;
}

.hero-panel {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 1.5rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid rgba(16, 42, 67, 0.08);
}

.eyebrow {
  margin: 0 0 0.5rem;
  font-size: 0.85rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.16em;
  color: #1d4ed8;
}

.hero-panel h1 {
  margin: 0;
  font-size: clamp(2rem, 2.8vw, 2.6rem);
  line-height: 1.05;
}

.hero-copy {
  margin: 1rem 0 0;
  max-width: 620px;
  color: #334e68;
  line-height: 1.75;
}

.hero-badge {
  min-width: 170px;
  padding: 1rem 1.25rem;
  border-radius: 24px;
  background: #ffffff;
  border: 1px solid rgba(16, 42, 67, 0.08);
  box-shadow: 0 10px 30px rgba(16, 42, 67, 0.08);
  display: grid;
  gap: 0.25rem;
  place-content: center;
  text-align: left;
}

.hero-badge span {
  font-size: 0.95rem;
  color: #475569;
}

.hero-badge strong {
  display: block;
  font-size: 1.7rem;
  color: #0f172a;
}

.layout-grid {
  display: grid;
  grid-template-columns: 1.05fr 0.95fr;
  gap: 1.25rem;
  margin-top: 1.75rem;
}

.input-panel,
.result-panel {
  background: #ffffff;
  border-radius: 28px;
  padding: 1.5rem;
  box-shadow: 0 18px 40px rgba(23, 43, 77, 0.08);
}

.field-row {
  display: grid;
  gap: 0.65rem;
  margin-bottom: 1.4rem;
}

.field-row label {
  font-size: 0.95rem;
  font-weight: 600;
  color: #102a43;
}

.field-row input {
  max-width: 100%;
  border: 1px solid #d9e2ec;
  border-radius: 16px;
  padding: 1rem 1.1rem;
  font-size: 1rem;
  color: #102a43;
  background: #f8fafc;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.field-row input:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 4px rgba(59, 130, 246, 0.12);
}

.toggle-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  padding: 1.25rem 1rem;
  border-radius: 22px;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
}

.toggle-label span {
  display: block;
  font-weight: 600;
  color: #102a43;
}

.toggle-label small {
  display: block;
  margin-top: 0.25rem;
  color: #64748b;
  line-height: 1.5;
}

.switch {
  position: relative;
  display: inline-flex;
  width: 52px;
  height: 28px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  inset: 0;
  border-radius: 999px;
  background: #cbd5e1;
  transition: background 0.2s ease;
}

.slider::before {
  content: '';
  position: absolute;
  left: 4px;
  top: 4px;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #ffffff;
  box-shadow: 0 2px 10px rgba(16, 42, 67, 0.12);
  transition: transform 0.2s ease;
}

.switch input:checked + .slider {
  background: #3b82f6;
}

.switch input:checked + .slider::before {
  transform: translateX(24px);
}

.result-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 1rem;
  margin-bottom: 1.25rem;
}

.result-label {
  margin: 0;
  font-size: 0.95rem;
  font-weight: 700;
  color: #102a43;
}

.result-status {
  text-align: center;
  display: inline-flex;
  padding: 0.5rem 0.85rem;
  border-radius: 999px;
  background: rgba(59, 130, 246, 0.1);
  color: #1e40af;
  font-size: 0.9rem;
  font-weight: 600;
}

.result-list {
  display: grid;
  gap: 1rem;
}

.result-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  padding: 1rem 1.15rem;
  border-radius: 20px;
  background: #f8fafc;
}

.result-item span {
  color: #475569;
}

.result-item strong {
  color: #102a43;
  font-size: 1rem;
}

.result-item.highlight {
  background: linear-gradient(90deg, rgba(59, 130, 246, 0.12), rgba(236, 248, 255, 0.8));
}

@media (max-width: 870px) {
  .layout-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 640px) {
  .calculator-card {
    padding: 1.5rem;
  }

  .hero-panel {
    flex-direction: column;
  }

  .hero-badge {
    width: 100%;
    justify-items: start;
  }

  .result-header {
    flex-direction: column;
    align-items: stretch;
  }
}
</style>
