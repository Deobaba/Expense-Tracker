

<template>
  <Header />
  <div class="container">
    <Balance :total = "+total" />
    <IncomeExpenses  :income="+income"  :expense="+expense"/>
    <TransactionList :transactions ="transactions"   @deleteTransaction ="handleDeleteTransaction"/>
    <AddTransaction @addtransaction = 'addTransaction'/>
  </div>
</template>

<script setup>
import Header from './components/Header.vue'
import Balance from './components/Balance.vue'
import IncomeExpenses from './components/IncomeExpenses.vue'
import TransactionList from './components/TransactionList.vue'
import AddTransaction from './components/AddTransaction.vue'
import { ref, computed } from 'vue'
import {useToast} from 'vue-toastification'
const toast = useToast()


const transactions =ref( [
  { id: 1, text: 'Flower', amount: -20 },
  { id: 2, text: 'Salary', amount: 300 },
  { id: 3, text: 'Book', amount: -10 },
  { id: 4, text: 'Camera', amount: 150 }])

const total = computed(()=>{
  return transactions.value.reduce((acc, transaction) => (acc += transaction.amount), 0).toFixed(2)
})

const income = computed(()=>{
  return transactions.value.filter(transaction => transaction.amount > 0).reduce((acc, transaction) => (acc += transaction.amount), 0).toFixed(2)
})

const expense = computed(()=>{
  return transactions.value.filter(transaction => transaction.amount < 0).reduce((acc, transaction) => (acc += transaction.amount), 0).toFixed(2)
})

const addTransaction = (transactionData) => {
  transactions.value.push({
    id: transactionData.id,
    text: transactionData.text,
    amount: transactionData.amount
  })

  toast.success('Transaction added successfully')
}

const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter(transaction => transaction.id !== id)
  toast.success('Transaction deleted successfully')
}



</script>