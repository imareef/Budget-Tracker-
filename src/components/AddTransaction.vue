<template>
    <div>
      <form @submit.prevent="onSubmit">
        <div class="mb-3">
          <label for="text" class="form-label">Title</label>
          <input
            v-model="text"
            type="text"
            class="form-control"
            id="text"
            placeholder="Example: Salary..."
            required
          />
        </div>
        <div class="mb-3">
          <label for="amount" class="form-label">Amount (positive = income, negative = expense)</label>
          <input
            v-model.number="amount"
            type="number"
            class="form-control"
            id="amount"
            placeholder="Example: 1000 or -500"
            required
          />
        </div>
        <button type="submit" class="btn btn-success w-100">Add New Transaction</button>
      </form>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const emit = defineEmits(['add-transaction']); 
  
 
  const text = ref('');
  const amount = ref('');
  
  const onSubmit = () => {
    if (!text.value || !amount.value) return;
  

    emit('add-transaction', {
      id: Date.now(), 
      text: text.value,
      amount: amount.value,
    });
  
    text.value = '';
    amount.value = '';
  };

  </script>
  
  <style scoped>

  </style>
  