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
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();
const transactions = ref([]);
onMounted(() => {
  // Load transactions from localStorage if available
  const storedTransactions = localStorage.getItem('transactions');
  if (storedTransactions) {
    transactions.value = JSON.parse(storedTransactions);
  }
});
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
  saveTransactionsToLocalStorage();
  toast.success('Transaction added successfully!');
};

// Delete transaction
const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter(
    (transaction) => transaction.id !== id,
  );
  saveTransactionsToLocalStorage();
  toast.success('Transaction deleted successfully!');
};
// Save transactions to localStorage whenever they change
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};
// Generate unique ID for transactions(helper function)
function generateUniqueId() {
  return Math.floor(Math.random() * 1000000);
}
</script>
