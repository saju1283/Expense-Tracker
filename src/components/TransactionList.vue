<template>
  <div>
    <h3 class="mb-3">Transaction List</h3>
    <select v-model="filter" class="form-select mb-3">
      <option value="All">All</option>
      <option value="Income">Income</option>
      <option value="Expense">Expense</option>
    </select>
    <table class="table table-bordered">
      <thead>
        <tr>
          <th>Title</th>
          <th>Amount</th>
          <th>Type</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(transaction, index) in filteredTransactions" :key="index">
          <td>{{ transaction.title }}</td>
          <td 
            :class="{
              'text-success': transaction.type === 'INCOME',
              'text-danger': transaction.type === 'EXPENSE',
              'fw-bold': transaction.amount >= 500
            }">
            ${{ transaction.amount }}
          </td>
          <td>{{ transaction.type }}</td>
          <td>
            <button class="btn btn-danger" @click="$emit('delete-transaction', index)">Delete</button>
          </td>
        </tr>
        <tr v-if="filteredTransactions.length === 0">
          <td colspan="4" class="text-center">No transactions recorded yet.</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  props: ['transactions'],
  emits: ['delete-transaction'],
  setup(props) {
    const filter = ref('All');

    const filteredTransactions = computed(() => {
      if (filter.value === 'All') return props.transactions;
      return props.transactions.filter(t => t.type === filter.value.toUpperCase());
    });

    return { filter, filteredTransactions };
  }
};
</script>
