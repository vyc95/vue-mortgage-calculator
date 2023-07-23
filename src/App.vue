<script setup>
import { ref, onMounted, computed } from "vue";
const purchasePrice = ref(0);
const downPayment = ref(0);
const repaymentTime = ref(0);
const interestRate = ref(0);
const loanAmount = computed(() => {
  const calculatedLoanAmount = purchasePrice.value - downPayment.value;
  return calculatedLoanAmount >= 0 ? calculatedLoanAmount : 0;
});

const downPaymentGreaterThanPurchase = computed(() => {
  return downPayment.value > purchasePrice.value;
});

const monthlyInterestRate = computed(() => {
  return interestRate.value / 100 / 12;
});

const totalPayments = computed(() => {
  return repaymentTime.value * 12;
});

const estimatedPerMonth = computed(() => {
  const numerator =
    loanAmount.value *
    monthlyInterestRate.value *
    Math.pow(1 + monthlyInterestRate.value, totalPayments.value);
  const denominator =
    Math.pow(1 + monthlyInterestRate.value, totalPayments.value) - 1;
  const monthlyPayment = numerator / denominator;
  return !isNaN(monthlyPayment) ? monthlyPayment.toFixed(2) : monthlyPayment;
});
</script>

<template>
  <div class="container mt-5">
    <h1 class="mb-4">Mortgage Calculator</h1>

    <div class="input-group mb-3">
      <label class="input-group-text" for="purchasePrice">Purchase Price ($):</label>
      <input
        type="range"
        inputmode="numeric"
        min="0"
        max="1000000"
        step="1000"
        v-model="purchasePrice"
        id="purchasePrice"
        class="form-range custom-range"
      />
      <span class="range-value input-group-text">${{ purchasePrice }}</span>
    </div>

    <div class="input-group mb-3">
      <label class="input-group-text" for="downPayment">Down Payment ($):</label>
      <input
        type="range"
        inputmode="numeric"
        :min="0"
        :max="purchasePrice"
        step="1000"
        v-model="downPayment"
        id="downPayment"
        class="form-range custom-range"
      />
      <span class="range-value input-group-text">${{ downPayment }}</span>
      <p v-if="downPaymentGreaterThanPurchase" class="error">Down payment cannot exceed the purchase price.</p>
    </div>

    <div class="input-group mb-3">
      <label class="input-group-text" for="repaymentTime">Repayment Time (Years):</label>
      <input
        type="range"
        inputmode="numeric"
        min="0"
        max="200"
        v-model="repaymentTime"
        id="repaymentTime"
        class="form-range custom-range"
      />
      <span class="range-value input-group-text">{{ repaymentTime }} years</span>
    </div>

    <div class="input-group mb-3">
      <label class="input-group-text" for="interestRate">Interest Rate (%):</label>
      <input
        type="range"
        inputmode="numeric"
        min="0"
        max="100"
        step="0.1"
        v-model="interestRate"
        id="interestRate"
        class="form-range custom-range"
      />
      <span class="range-value input-group-text">{{ interestRate }}%</span>
    </div>

    <div class="result">
      <div v-if="loanAmount">Loan Amount: ${{ loanAmount }}</div>
      <div v-if="estimatedPerMonth">Estimated Monthly Payment: ${{ estimatedPerMonth }}</div>
    </div>
  </div>
</template>

<style scoped>
/* Add style for range sliders and range values */

.error {
  color: red;
}

.input-group input[type="range"] {
  width: 100%;
}

.range-value {
  display: block;
  text-align: center;
}

/* Custom styles for the range slider track and thumb */
.custom-range::-webkit-slider-runnable-track {
  background-color: #007bff;
  height: 4px;
}

.custom-range::-moz-range-track {
  background-color: #007bff;
  height: 4px;
}

.custom-range::-webkit-slider-thumb {
  background-color: #007bff;
  border: 2px solid #007bff;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  cursor: pointer;
  -webkit-appearance: none;
}

.custom-range::-moz-range-thumb {
  background-color: #007bff;
  border: 2px solid #007bff;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  cursor: pointer;
}

/* Additional styles for the range slider labels */
.input-group .input-group-text {
  min-width: 100px;
}
</style>
