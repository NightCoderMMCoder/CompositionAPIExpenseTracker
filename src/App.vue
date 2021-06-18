<template>
  <TheHeader />
  <div class="container">
    <CalculateTransaction :transactions="transactions" />
    <TransactionsList :transactions="transactions" />
    <AddTransaction @add-transaction="addTransaction" />
  </div>
</template>

<script>
import { ref } from "vue";
import AddTransaction from "./components/AddTransaction.vue";
import CalculateTransaction from "./components/CalculateTransaction.vue";
import TheHeader from "./components/Layouts/TheHeader.vue";
import TransactionsList from "./components/TransactionsList.vue";
export default {
  name: "App",
  components: {
    TheHeader,
    CalculateTransaction,
    TransactionsList,
    AddTransaction,
  },
  setup() {
    const transactions = ref([
      {
        id: 1,
        name: "Cash",
        amount: -400,
      },
      {
        id: 2,
        name: "Salary",
        amount: 100,
      },
      {
        id: 3,
        name: "Computer",
        amount: -10000,
      },
    ]);

    const addTransaction = (transaction) => {
      const newTransaction = {
        id: Math.floor(Math.random() * 10000000),
        ...transaction,
      };
      transactions.value.push(newTransaction);
    };

    return { transactions, addTransaction };
  },
};
</script>
