<template>
  <form @submit.prevent="handleSubmit" class="mb-4">
    <div class="mb-3">
      <label class="form-label">Title</label>
      <input v-model="title" type="text" class="form-control" required />
    </div>
    <div class="mb-3">
      <label class="form-label">Amount</label>
      <input v-model.number="amount" type="number" class="form-control" required min="1" />
    </div>
    <div class="mb-3">
      <label class="form-label">Type</label>
      <select v-model="type" class="form-select" required>
        <option value="Income">Income</option>
        <option value="Expense">Expense</option>
      </select>
    </div>
    <button type="submit" class="btn btn-primary">Add Transaction</button>
  </form>
</template>

<script>
import { ref } from 'vue';

export default {
  emits: ['add-transaction'],
  setup(_, { emit }) {
    const title = ref('');
    const amount = ref('');
    const type = ref('Income');

    const handleSubmit = () => {
      if (!title.value || amount.value <= 0) return;
      emit('add-transaction', {
        title: title.value,
        amount: amount.value,
        type: type.value.toUpperCase()
      });
      title.value = '';
      amount.value = '';
      type.value = 'Income';
    };

    return { title, amount, type, handleSubmit };
  }
};
</script>
