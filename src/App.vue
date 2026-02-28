<template>
  <Header />
  <div class="contain">
    <Balance :total="total" />
    <IncomeExpense :income="income" :expense="expense" />
    <TransactionList
      :transactions="transactions"
      @delete-transaction="deleteTransaction" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();
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
//  Add new transaction
const handleTransactionSubmitted = (newTransaction) => {
  transactions.value.push({
    id: generateUniqueId(),
    ...newTransaction,
  });
  toast.success('Transaction added successfully!');
};

// Delete transaction
const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id,
  );
  toast.success('Transaction deleted successfully!');
};
// Generate unique ID for transactions(helper function)
function generateUniqueId() {
  return Math.floor(Math.random() * 1000000);
}
</script>
