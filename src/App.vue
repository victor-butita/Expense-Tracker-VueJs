<template>
	<Header />
	<div class="container">
		<Balance :total="+total" />
		<IncomeExpenses :income="+income" :expenses="+expenses" />
		<TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
		<AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
	</div>
</template>

<script setup>
	import Header from './components/Header.vue'
	import Balance from './components/Balance.vue'
	import IncomeExpenses from './components/IncomeExpenses.vue'
	import TransactionList from './components/TransactionList.vue'
	import AddTransaction from './components/AddTransaction.vue'
	import {useToast} from 'vue-toastification';

	import { ref, computed } from 'vue'

	const toast = useToast;
	const transactions = ref([
		{ id: 1, text: 'Flower', amount: -19.99 },
		{ id: 2, text: 'Salary', amount: 699.97 }
	])

	//Get Total
	const total = computed(() => {
		return transactions.value.reduce((acc, transaction) => {
			return acc + transaction.amount
		}, 0)
	})

	//Get Income
	const income = computed(() => {
		return transactions.value
			.filter((transaction) => transaction.amount > 0)
			.reduce((acc, transaction) => {
				return acc + transaction.amount
			}, 0)
			.toFixed(2)
	})

	//Get expenses
	const expenses = computed(() => {
		return transactions.value
			.filter((transaction) => transaction.amount < 0)
			.reduce((acc, transaction) => {
				return acc + transaction.amount
			}, 0)
			.toFixed(2)
	})

	//Add transaction

	const handleTransactionSubmitted = (transactionData) => {
		transactions.value.push({
			id: generateUniqueId(),
			text: transactionData.text,
			amount: transactionData.amount
		})

		toast.success('Transaction added');
	};

	//Generate unique ID

	const generateUniqueId = () => {
		return Math.floor(Math.random() * 1000000)
	}

	//Delete transaction
	const handleTransactionDeleted = (id) =>{
		transactions.value = transactions.value.filter((transaction) =>
		transactions.id !== id);
		
		toast.success('Transaction deleted');
	};
</script>
