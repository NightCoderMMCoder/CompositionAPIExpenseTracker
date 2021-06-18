<template>
  <TheHeader />
  <div class="container">
    <CalculateTransaction />
    <TransactionsList />
    <AddTransaction />
  </div>
</template>

<script>
import AddTransaction from "./components/AddTransaction.vue";
import CalculateTransaction from "./components/CalculateTransaction.vue";
import TheHeader from "./components/Layouts/TheHeader.vue";
import TransactionsList from "./components/TransactionsList.vue";
export default {
  components: {
    TheHeader,
    CalculateTransaction,
    TransactionsList,
    AddTransaction,
  },
  name: "App",
};
</script>
