<template>
  <div class="container mt-4">
    <h2 class="text-center">Expense Tracker</h2>
    <AddTransaction @add-transaction="addTransaction" />
    <TransactionList :transactions="transactions" @delete-transaction="deleteTransaction" />
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import AddTransaction from './components/AddTransaction.vue';
import TransactionList from './components/TransactionList.vue';

export default {
  components: { AddTransaction, TransactionList },
  setup() {
    const transactions = ref(JSON.parse(localStorage.getItem('transactions')) || []);

    const saveTransactions = () => {
      localStorage.setItem('transactions', JSON.stringify(transactions.value));
    };

    const addTransaction = (transaction) => {
      transactions.value.push(transaction);
      saveTransactions();
    };

    const deleteTransaction = (index) => {
      transactions.value.splice(index, 1);
      saveTransactions();
    };

    return { transactions, addTransaction, deleteTransaction };
  }
};
</script>
