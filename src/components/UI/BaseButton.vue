<template>
  <button>
    <slot></slot>
  </button>
</template>

<script>
export default {};
</script>

<style scoped>
button {
  background: #9c88ff;
  color: white;
  padding: 8px 20px;
  border-radius: 100px;
  border: 2px solid #9c88ff;
  cursor: pointer;
  font-weight: bold;
}
</style>
