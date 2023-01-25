<template>
  <div>
    <input class='input' :value="modelValue" @input="$emit('update:modelValue', $event.target.value)" />
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'my-input',
  props: ['modelValue'],
  emits: ['update:modelValue']
}
</script>

<style lang="scss" scoped>
$blue: rgb(19, 140, 196);


.input {
  border: 2px solid $blue;
  padding: 10px 20px;
  border-radius: 5px;
  margin-bottom: auto;
  width: 250px;
}
</style>