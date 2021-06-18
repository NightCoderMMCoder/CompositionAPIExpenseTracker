<template>
  <h3>History</h3>
  <transition-group name="slide" tag="ul">
    <TransactionItem
      v-for="transaction in transactions"
      :key="transaction.id"
      :transaction="transaction"
    />
  </transition-group>
</template>

<script>
import TransactionItem from "./TransactionItem.vue";
export default {
  components: { TransactionItem },
  props: { transactions: Array },
};
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
  margin-bottom: 40px;
  position: relative;
}
.slide-enter-from {
  transform: translateX(-100px);
  opacity: 0;
}
.slide-leave-to {
  transform: translateX(100px);
  opacity: 0;
}
.slide-leave-active {
  position: absolute;
}
</style>
