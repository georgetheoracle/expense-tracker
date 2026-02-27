<template>
  <Header />
  <div class="contain">
    <Balance :total="total" />
    <IncomeExpense :income="income" :expense="expense" />
    <TransactionList :transactions="transactions" />
    <AddTransaction />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed } from 'vue';

const transactions = ref([
  { id: 1, text: 'Flower', amount: -19.99 },
  { id: 2, text: 'Salary', amount: 299.97 },
  { id: 3, text: 'Groceries', amount: -150 },
  { id: 4, text: 'Freelance Work', amount: 500 },
]);

// Calculate total balance
const total = computed(() => {
  return transactions.value.reduce(
    (acc, transaction) => acc + transaction.amount,
    0,
  );
});
//   Get total income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

//   Get total expense
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});
</script>
