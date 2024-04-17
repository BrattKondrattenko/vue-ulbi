<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'my-button'
}
</script>

<style scoped>

.btn {
  border: 1px solid teal;
  padding: 10px 15px;
  align-self: flex-end;
  margin-bottom: 15px;
  background: none;
  color: teal;
  cursor: pointer;
}
</style>