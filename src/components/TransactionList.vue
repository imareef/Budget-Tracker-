<template>
  <div>
    <ul class="list-group">
      <li
        v-for="transaction in transactions"
        :key="transaction.id"
        class="list-group-item d-flex justify-content-between align-items-center"
        :class="transaction.amount < 0 ? 'list-group-item-danger' : 'list-group-item-success'"
      >
        <span>{{ transaction.text }}</span>
        <span>
          {{ formatCurrency(transaction.amount) }}
          <button
            class="btn btn-sm btn-danger ms-3"
            @click="emit('delete-transaction', transaction.id)"
          >
            Delete
          </button>
        </span>
      </li>
    </ul>
  </div>
</template>

<script setup>

defineProps({
  transactions: Array, 
});

const emit = defineEmits(['delete-transaction']);

const formatCurrency = (amount) => {
  return amount.toLocaleString() + ' IRT';
};
</script>

<style scoped>
.list-group-item {
  font-size: 1rem;
  font-weight: 500;
}
</style>
