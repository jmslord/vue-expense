<template>
  <Header />
  <div class="container">
    <Balance :total="+total"/>
    <IncomeExpense :income="+income" :expenses="+expenses" />
    <TransactionList :transactions="transactions" @delete-transaction="handleDeleteTransaction"/>
    <AddTransaction @add-transaction="handleAddTransaction"/>
  </div>
 
</template>

<script setup>
  import Header from './components/Header.vue'
  import Balance from './components/Balance.vue'
  import IncomeExpense from './components/IncomeExpense.vue'
  import TransactionList from './components/TransactionList.vue'
  import AddTransaction from './components/AddTransaction.vue'

  import {useToast} from 'vue-toastification'

  import { ref,computed, onMounted } from 'vue'
  
  const Toast = useToast()

  //Transactions
  const transactions= ref([]);

  onMounted(()=>{
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
    if (savedTransactions) {
      transactions.value = savedTransactions;
    }
  })

  //Get Total
  const total= computed(()=>{
    return transactions.value.reduce((acc, item) => (acc += item.amount), 0).toFixed(2);
  })
  //Get Income
  const income= computed(()=>{
    return transactions.value
      .filter(transaction => transaction.amount > 0)
      .reduce((acc, transaction) => (acc += transaction.amount), 0)
      .toFixed(2);
  })
  //Get Expenses
  const expenses= computed(()=>{
    return (
      transactions.value
        .filter(transaction => transaction.amount < 0)
        .reduce((acc, transaction) => (acc += transaction.amount), 0) * -1
    ).toFixed(2);
  })
  //Add Transaction
  const handleAddTransaction=(transactionData)=>{
    transactions.value.push(
      {
        id:generateUniqueId(),
        text:transactionData.text,
        amount:transactionData.amount
      }
    )
    saveToLocalStorage();
    Toast.success('Transaction Added')
  }
  //Generate Unique ID
  const generateUniqueId=()=>{
    return Math.floor(Math.random() * 100000000);
  }
  //Delete Transaction
  const handleDeleteTransaction = (id) => {
    transactions.value = transactions.value.filter(transaction => transaction.id !== id);
    saveToLocalStorage();
    Toast.error('Transaction Deleted')
  }

  //Save to Local Storage
  const saveToLocalStorage=()=>{
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
  }

</script>

<!-- <script>
  import Header from './components/Header.vue'
  import Balance from './components/Balance.vue'
  import IncomeExpense from './components/IncomeExpense.vue'
  import TransactionList from './components/TransactionList.vue'
  import AddTransaction from './components/AddTransaction.vue'
  export default {
    components: {
      Header,
      Balance,
      IncomeExpense,
      TransactionList,
      AddTransaction
    }
  }
</script> -->