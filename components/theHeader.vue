<script>
export default {
  data() {
    return {
      toggleNavi: false
    }
  },
  
  methods: {
    activeToggleNavi() {
      this.toggleNavi = !this.toggleNavi
    }
  }
}
</script>

<template>
  <header class="header">
    <div class="header__main">
      <h1><a href="/">Customer Products</a></h1>
      <MobileBurguerButton @click="activeToggleNavi" />
    </div>
    <nav :class="{'showNavi' :  toggleNavi, 'header__navi' : true}">
      <ul class="header__list">
        <li><a class="list__item" href="/customer-form">Customers</a></li>
        <li><a class="list__item" href="/product-form">Product</a></li>
      </ul>
    </nav>
  </header>
</template>

<style lang="scss">

.header {
  &__main {
    display: flex;
    align-items: center;
    justify-content: space-around;
  }
  
  &__navi {
    height: 0px;
    display: flex;
    align-content: center;
    justify-content: center;
  }

  &__list {
    font-size: 20px;
    display: flex;
    overflow: hidden;
    margin: 0px;
    padding: 0;
    display: flex;
    align-content: center;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    list-style: none;
  }
}

a {
  text-decoration: none;
  color: white;
}

.showNavi {
  height: auto;
}

@media(min-width: 1000px) {

  .header {
    position: relative;

    &__main {
      justify-content: start;
    }

    &__navi {
      position: absolute;
      height: auto;
      top: 30px;
      right: 9px;
      
    }

    &__list {
      flex-direction: row;
      justify-content: space-between;
    }

  }

  .list__item {
    margin-left: 30px;
  } 
}
</style>