<template>
  <button class="mobileButton">
    <span class=mobileButton__line></span>
    <span class=mobileButton__line></span>
    <span class=mobileButton__line></span>
  </button>
</template>

<style lang="scss">
.mobileButton {
  cursor: pointer;
  align-content: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0);
  border: solid 3px white;
  border-radius: 5px;
  height: 40px;
  width: 55px;

  &__line {
    margin: 3px auto;
    border-bottom: solid 1px white;
    width: 25px;
    height: 1px;
    background-color: white;
  }
}

@media(min-width: 1000px) {
  .mobileButton {
    display: none;
  }
}
</style>