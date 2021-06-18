<template>
  <TheHeader />
  <BaseDialog :show-model="showModel">
    <template #header>
      Just Checking...
    </template>
    <template #body>
      <p>Are you sure to delete?</p>
    </template>
    <template #actions>
      <BaseButton class="flat" @click="confirmDelete">Close</BaseButton>
      <BaseButton @click="removeTransaction">Ok</BaseButton>
    </template>
  </BaseDialog>
  <div class="container">
    <CalculateTransaction :transactions="transactions" />
    <TransactionsList :transactions="transactions" />
    <AddTransaction @add-transaction="addTransaction" />
  </div>
</template>

<script>
import { provide, ref } from "vue";
import AddTransaction from "./components/AddTransaction.vue";
import CalculateTransaction from "./components/CalculateTransaction.vue";
import TheHeader from "./components/Layouts/TheHeader.vue";
import TransactionsList from "./components/TransactionsList.vue";
import BaseDialog from "./components/UI/BaseDialog.vue";
import BaseButton from "./components/UI/BaseButton.vue";
export default {
  name: "App",
  components: {
    TheHeader,
    CalculateTransaction,
    TransactionsList,
    AddTransaction,
    BaseDialog,
    BaseButton,
  },
  setup() {
    const transactions = ref([]);

    const getTransactions = async () => {
      const res = await fetch("http://localhost:3000/transactions");
      const data = await res.json();
      transactions.value = data.reverse();
    };
    getTransactions();

    const addTransaction = async (transaction) => {
      const newTransaction = {
        id: Math.floor(Math.random() * 10000000),
        ...transaction,
      };
      transactions.value.unshift({ ...newTransaction });
      delete newTransaction.id;
      await fetch("http://localhost:3000/transactions", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(newTransaction),
      });
    };

    const showModel = ref(false);
    const transactionId = ref(null);

    const removeTransaction = () => {
      const idx = transactions.value.findIndex(
        (transaction) => transaction.id === transactionId.value
      );
      transactions.value.splice(idx, 1);
      fetch(`http://localhost:3000/transactions/${transactionId.value}`, {
        method: "DELETE",
      });
      confirmDelete();
    };
    const confirmDelete = (id) => {
      transactionId.value = id;
      showModel.value = !showModel.value;
    };

    provide("removeTransaction", confirmDelete);

    return {
      transactions,
      addTransaction,
      showModel,
      removeTransaction,
      confirmDelete,
    };
  },
};
</script>

<style scoped>
p {
  margin-top: 0;
}
</style>
