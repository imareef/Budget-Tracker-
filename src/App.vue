<template>
    <div class="container mt-5">
        <h1 class="text-center">Budget Tracker</h1>
        <hr />
        <div class="toolbar">
            <button
                class="btn btn-success"
                data-bs-toggle="modal"
                data-bs-target="#addTransactionModal"
            >
                Add Transaction
            </button>
            <div class="search-container">
                <input
                    type="text"
                    class="form-control"
                    placeholder="Search..."
                    v-model="searchQuery"
                />
            </div>
        </div>

        <hr />

        <div
            v-if="transactions.length === 0"
            class="alert alert-warning text-center"
        >
            No transactions have been recorded.
        </div>
        <div v-else>
            <TransactionList
                :transactions="filteredTransaction"
                @delete-transaction="deleteTransaction"
            />
        </div>

        <hr />
        <div class="alert d-flex justify-content-between" :class="balance < 0 ? 'alert-danger' : 'alert-info'">
            <h4>Income: {{ formatCurrency(totalIncome) }}</h4>
            <h4>Balance: {{ formatCurrency(balance) }}</h4>
            <h4>Expence: {{ formatCurrency(totalExpence) }}</h4>
        </div>

        <div
            class="modal fade"
            id="addTransactionModal"
            tabindex="-1"
            aria-labelledby="addTransactionModalLabel"
            aria-hidden="true"
        >
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="addTransactionModalLabel">
                            Add Transaction
                        </h5>
                        <button
                            type="button"
                            class="btn-close"
                            data-bs-dismiss="modal"
                            aria-label="Close"
                        ></button>
                    </div>
                    <div class="modal-body">
                        <AddTransaction @add-transaction="addTransaction" />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from "vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

const transactions = ref([
    // { id: 1, text: "حقوق", amount: 1000 }
    // { id: 2, text: "غذا", amount: -200 },
    // { id: 3, text: "کرایه خانه", amount: -500 },
    // { id: 4, text: "فروش لپتاپ", amount: 800 },
]);

const searchQuery = ref("");

const filteredTransaction = computed(() => {
    if (!searchQuery.value.trim()) {
        return transactions.value;
    }
    return transactions.value.filter((transaction) =>
        transaction.text
            .toLowerCase()
            .includes(searchQuery.value.trim().toLowerCase())
    );
});

const balance = computed(() => {
    return transactions.value.reduce(
        (acc, transaction) => acc + transaction.amount,
        0
    );
});

const totalIncome = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0);
});

const totalExpence = computed(() => {
    return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((acc, transaction) => acc + transaction.amount, 0);
});

const addTransaction = (transaction) => {
    transactions.value.push(transaction);
};

const deleteTransaction = (id) => {
    transactions.value = transactions.value.filter(
        (transaction) => transaction.id !== id
    );
};

const formatCurrency = (amount) => {
    return amount.toLocaleString() + " IRT";
};

watch(balance, (newBalance) => {
  if (newBalance < 0) {
    console.warn('The overall balance became negative!');
  }
});

onMounted(() => {
  console.log("The budget tracker app has been loaded.");
});

</script>

<style scoped>
h1 {
    margin-bottom: 20px;
}

.toolbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    /* border: 1px solid #ccc; */
    background-color: #f9f9f9;
}

.search-container {
    display: flex;
    gap: 5px;
}
</style>
