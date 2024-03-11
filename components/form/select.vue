<script>
export default {
  methods: {
    handleSelectChange(event) {
      this.$emit('update:modelValue', event.target.value);
      console.log(event.target.value)
    }
  }
}
</script>

<template>
  <select @change="handleSelectChange">
    <slot />
  </select>
</template>

<style scoped lang="scss">
select {
  min-width: 250px;
  border: solid 2px white;
  border-radius: 3px;
  background: orangered;
  color: white;
}
</style>