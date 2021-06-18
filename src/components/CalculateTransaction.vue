<template>
  <h4>Your Balance</h4>
  <h1 id="balance">{{ calcTotalBalance }} ks</h1>

  <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <p id="money-plus" class="money plus">
        {{ calcIncome !== "0.00" ? "+" : "" }}{{ calcIncome }} ks
      </p>
    </div>
    <div>
      <h4>Expense</h4>
      <p id="money-minus" class="money minus">{{ calcExpense }} ks</p>
    </div>
  </div>
</template>

<script>
import { computed } from "vue";
export default {
  props: { transactions: Array },
  setup(props) {
    const calcTotalBalance = computed(() => {
      let total = props.transactions.reduce(
        (total, cur) => total + cur.amount,
        0
      );
      return formatCurrency(total);
    });

    const calcIncome = computed(() => {
      const incomeArray = props.transactions.filter(
        (transaction) => transaction.amount > 0
      );
      const total = incomeArray.reduce((total, cur) => total + cur.amount, 0);
      return formatCurrency(total);
    });

    const calcExpense = computed(() => {
      const expenseArray = props.transactions.filter(
        (transaction) => transaction.amount < 0
      );
      const total = expenseArray.reduce((total, cur) => total + cur.amount, 0);
      return formatCurrency(total);
    });

    const formatCurrency = (number) => {
      return number.toFixed(2).replace(/\d(?=(\d{3})+\.)/g, "$&,");
    };
    return { calcTotalBalance, calcIncome, calcExpense };
  },
};
</script>

<style scoped>
.inc-exp-container {
  background-color: #fff;
  box-shadow: var(--box-shadow);
  padding: 20px;
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
}

.inc-exp-container > div {
  flex: 1;
  text-align: center;
}

.inc-exp-container > div:first-of-type {
  border-right: 1px solid #dedede;
}

.money {
  font-size: 20px;
  letter-spacing: 1px;
  margin: 5px 0;
}

.money.plus {
  color: #2ecc71;
}

.money.minus {
  color: #c0392b;
}
</style>
