<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses
      :totalIncome="+totalIncome"
      :totalExpenses="+totalExpenses"
    />
    <TransactionList
      @removeTransaction="handleremoveTransaction"
      :transactions="transactions"
    />
    <AddTransaction @submitted="handleSubmittedEmit" />
  </div>
</template>

<script setup>
import { computed, ref, onMounted } from "vue";
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const savedTaransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTaransactions) {
    transactions.value = savedTaransactions;
  }
});

const total = computed(() => {
  return transactions.value.reduce((acc, curr) => acc + curr.amount, 0);
});

const totalIncome = computed(() => {
  return transactions.value
    .filter((el) => el.amount > 0)
    .reduce((acc, curr) => acc + curr.amount, 0)
    .toFixed(2);
});

const totalExpenses = computed(() => {
  const amount = transactions.value
    .filter((el) => el.amount < 0)
    .reduce((acc, curr) => acc + curr.amount, 0)
    .toFixed(2);

  return String(amount).slice(1);
});

const handleSubmittedEmit = (data) => {
  console.log("handleSubmittedEmit", data);

  transactions.value.push({
    id: Math.floor(Math.random() * 100000),
    text: data.text,
    amount: data.amount,
  });

  saveToLocalStorage();

  const toastMessage = data.amount < 0 ? "Expense is added" : "Income is added";

  toast.success(toastMessage);
};

const handleremoveTransaction = (id) => {
  console.log(id, transactions.value);
  const updatedTransactions = transactions.value.filter((el) => el.id !== id);
  transactions.value = updatedTransactions;
  saveToLocalStorage();
  toast.success("Transaction deleted");
};

const saveToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>

<style lang="scss" scoped></style>
