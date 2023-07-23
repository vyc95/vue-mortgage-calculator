<script setup>
import { ref, onMounted, computed } from "vue";
const purchasePrice = ref(0);
const downPayment = ref(0);
const repaymentTime = ref(0);
const interestRate = ref(0);
const loanAmount = computed(() => {
  return purchasePrice.value - downPayment.value;
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
  return !isNaN(monthlyPayment)?monthlyPayment.toFixed(2):monthlyPayment;
});
</script>

<template>
  <div class="container">
    <h1>Mortgage Calculator</h1>
    <input type="text" v-model="purchasePrice" />
    <input type="text" v-model="downPayment" />
    <input type="text" v-model="repaymentTime" />
    <input type="text" v-model="interestRate" />
    <div v-if="loanAmount">{{ loanAmount }}</div>
    <div v-if="estimatedPerMonth">{{ estimatedPerMonth }}</div>
  </div>
</template>

<style scoped>
.container {
  text-align: center;
}
</style>
