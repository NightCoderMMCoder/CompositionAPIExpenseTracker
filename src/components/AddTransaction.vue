<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="addTransaction">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        type="text"
        id="text"
        placeholder="Enter text..."
        v-model.trim="name"
        @blur="clearValidation('name')"
      />
      <small v-if="errors.name" class="error">{{ errors.name }}</small>
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="number"
        id="amount"
        placeholder="Enter amount..."
        v-model.number="amount"
        @blur="clearValidation('amount')"
      />
      <small v-if="errors.amount" class="error">{{ errors.amount }}</small>
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script>
import { reactive, toRefs, ref } from "vue";
export default {
  emits: ["addTransaction"],
  setup(_, { emit }) {
    const transaction = reactive({
      name: "",
      amount: "",
    });
    const errors = ref({});

    const addTransaction = () => {
      const formValidate = validation();
      if (formValidate) {
        emit("addTransaction", transaction);
        transaction.name = "";
        transaction.amount = "";
      }
    };

    const clearValidation = (key) => {
      if (transaction[key]) {
        errors.value[key] = "";
      }
    };

    const validation = () => {
      let isValidate = true;
      for (let [key, value] of Object.entries(transaction)) {
        if (!value) {
          isValidate = false;
          errors.value[key] = `Please fill the ${key} field.`;
        }
      }
      return isValidate;
    };

    return { ...toRefs(transaction), addTransaction, errors, clearValidation };
  },
};
</script>

<style scoped>
label {
  display: inline-block;
  margin: 10px 0;
}

input[type="text"],
input[type="number"] {
  border: 1px solid #dedede;
  border-radius: 2px;
  display: block;
  font-size: 16px;
  padding: 10px;
  width: 100%;
}
</style>
