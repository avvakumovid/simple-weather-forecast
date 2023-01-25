<template>
  <button class='btn'>
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'my-button'
}
</script>

<style lang="scss" scoped>
$blue: rgb(19, 140, 196);

.btn {
  border: 2px solid $blue;
  background-color: transparent;
  padding: 10px 20px;
  border-radius: 5px;
  color: $blue;
  text-transform: uppercase;
  font-weight: 700;
  margin-bottom: auto;
  max-width: 250px;
}
</style>