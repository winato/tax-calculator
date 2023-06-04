<template>
  <div class="calculator">
    <h1 class="title">
      Calculator
    </h1>
    <p  class="description">
      If you have some shares here you can try to calculate the total amount you have
    </p>
    <form class="form">
      <div class="input-wrapper">
        <label>
          Shares Amount
          <input 
            type="number"
            v-model="sharesAmount" 
            min="0"
          >
        </label>
      </div>
      <div class="input-wrapper">
        <label>
          Share Price
          <input
            type="number"
            min="0"
            v-model="sharePrice"
          >
        </label>
      </div>
      <div class="input-wrapper">
        <label>
          Has Ruling
          <input
            type="checkbox"
            min="0"
            v-model="hasRuling"
          >
        </label>
      </div>
    </form>
    <p>
      Income: <i>{{ income }}</i>
    </p>
    <p>
      Ruling Amount: <i>{{ minusRuling }}</i>
    </p>
    <p>
      Result: <b>{{ result }}</b>
    </p>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const sharesAmount = ref(0);
const sharePrice = ref(0);
const hasRuling = ref(true);

const income = computed(() => (sharesAmount.value * sharePrice.value).toFixed(2));
const minusRuling = computed(() => (income.value * (hasRuling.value ? 0.33 : 0)).toFixed(2));
const taxableIncome = computed(() => income.value - minusRuling.value);
const incumeTax = computed(() => taxableIncome.value * 0.56);
const result = computed(() => (income.value - incumeTax.value).toFixed(2));

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .calculator {
    max-width: 720px;
    margin: 0 auto;
    text-align: center;
  }

  .input-wrapper {
    margin: 5px;
  }
  
</style>
