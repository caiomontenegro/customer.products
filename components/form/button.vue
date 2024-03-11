<template>
  <button class="formButton">
    <slot />
  </button>
</template>

<style lang="scss">
.formButton {
  cursor: pointer;
  color: white;
  background-color: orangered;
  border: solid 2px white;
  border-radius: 3px;

  &--blue {
    color: white;
    background-color: rgb(94, 60, 173);
  }

  &--red {
    color: white;
    background-color: rgb(168, 2, 2);
  }
}
</style>