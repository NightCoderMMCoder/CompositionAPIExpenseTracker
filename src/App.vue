<template>
  <TheHeader />
  <BaseDialog v-if="showModel">
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

    const showModel = ref(false);
    const transactionId = ref(null);

    const removeTransaction = () => {
      const idx = transactions.value.findIndex(
        (transaction) => transaction.id === transactionId.value
      );
      transactions.value.splice(idx, 1);
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
