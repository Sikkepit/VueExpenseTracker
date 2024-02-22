<script setup>
defineProps(['transactions'])

const emit = defineEmits(['transactionDeleted'])

const deleteTransaction = (id) => {
  emit('transactionDeleted', id)
}
</script>

<template>
    <h3>History</h3>
    <ul id="list" class="list">
      <li 
      v-for="(transaction, index) in transactions" 
      :key="index"
      :class="transaction.amount < 0 ? 'minus' : 'plus'">
        {{ transaction.text }}<span>${{ transaction.amount }}</span
        ><button 
        @click="deleteTransaction(index)"
        class="delete-btn">
          x
        </button>
      </li>
    </ul>
  </template>

