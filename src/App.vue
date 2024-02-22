<script setup>
  import { ref, computed, onMounted } from 'vue';
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';


 const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if(savedTransactions) {
    transactions.value =  savedTransactions;
  }
})

  const total = computed(() => {
    let sum = 0;
    transactions.value.map(transaction => {
      sum +=transaction.amount
    })
    return sum;
  })

  const income = computed(() => {
    let sum = 0;
    transactions.value.map(transaction => {
      if (transaction.amount > 0) sum+=transaction.amount
    })
    return sum
  })

  const expenses = computed(() => {
    let sum = 0;
    transactions.value.map(transaction => {
      if (transaction.amount < 0) sum+=transaction.amount
    })
    return sum
  })

  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      text: transactionData.text,
      amount: transactionData.amount
    })
    saveTransactionsToLocalStorage();
  }

  const handleTransactionDeleted = (index) => {
    transactions.value.splice(index, 1)
    saveTransactionsToLocalStorage();
  }

// Update localStorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<template>
<Header></Header>
<div class="container">
  <Balance :total="+total"/>
  <IncomeExpenses :income="+income" :expense="+expenses"/>
  <TransactionList @transaction-deleted="handleTransactionDeleted" :transactions="transactions"/>
  <AddTransaction @transaction-submitted="handleTransactionSubmitted" />
</div>
</template>

<style scoped>
</style>
