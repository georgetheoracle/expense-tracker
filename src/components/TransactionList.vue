<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'">
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button class="delete-btn" @click="deleteTransaction(transaction.id)">
        x
      </button>
    </li>
  </ul>
</template>

<script setup>
import { defineProps } from 'vue';
const emit = defineEmits(['delete-transaction']);
const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const deleteTransaction = (id) => {
  // Emit an event to the parent component to delete the transaction
  emit('delete-transaction', id);
};
</script>
