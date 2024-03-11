<script>
import axios from 'axios'
export default {

  data() {
    return {
      customers: [],
      products: [],
      productsLinked: null,
      productId: null,
      customerSelected: null
    }
  },

  async beforeMount() {
    try {
      const responseCustomers = await axios.get('https://customer-products-test-default-rtdb.firebaseio.com/customer.json')
      this.customers = responseCustomers.data
      console.log('Customers: ', this.customers)

      const responseProducts = await axios.get('https://customer-products-test-default-rtdb.firebaseio.com/product.json')
      this.products = responseProducts.data

    } catch (err) {
      console.error('Errro on GET request products: ', err)
    }
  },

  methods: {
    selectCustomer(id) {
      if(this.customerSelected === id ) {
        this.customerSelected = null
      } else {
        this.customerSelected = id
      }
    },

    async addItem(customerId) {
      const currentProducts = await this.customers[customerId].products || {}
      const newProduct = await {[this.productId] : this.products[this.productId]}
      console.log('Produtos cadastrados: ', currentProducts)
      console.log('Novo produto: ', this.productId)
      try {
        const updatedProducts = { ...currentProducts, ...newProduct }
        console.log('Nova lista de produtos: ', updatedProducts)
        const responseAddItem = await axios.patch(`https://customer-products-test-default-rtdb.firebaseio.com/customer/${customerId}.json`, {
          products: updatedProducts
        })
        if (responseAddItem.status === 200) {
          location.reload()
        }
      } catch(err) {
        console.error("Error to add new item: ", err)
      }
    },

    async removeItem(productId, customerId) {
      const currentProducts = this.customers[customerId].products || {};
      const updatedProducts = { ...currentProducts }
      delete updatedProducts[productId]

      try {
        const responseRemoveItem = await axios.patch(`https://customer-products-test-default-rtdb.firebaseio.com/customer/${customerId}.json`, {
          products: updatedProducts
        })
      if (responseRemoveItem.status === 200) {
        location.reload()
      }
      } catch(err) {
        console.error('Error to delete product :', err)
      }
    }
  }
}
</script>

<template>
  <div class="title">
    <h2>Link Itens Micro Service</h2>
  </div>
  <section class="customer">
    <div class="customer__instruction">
      <span>Click on customer to expand him</span>
    </div>
    <div v-for="(customer, customerIndex) in customers" class="customer__box">
      <div v-if="customer.active">
        <div class="customer__name" @click="selectCustomer(customerIndex)">{{ customer.name }}</div>
        <div :class="{customer__data : true, showData : customerSelected === customerIndex}">
          <div class="customer__itemsList">
            <div v-if="customer.products" v-for="(productLinked, productLinkedIndex) in customer.products" :key="productLinkedIndex" class="customer__item">
              <span>{{ productLinked.name }}</span>
              <FormButton class="formButton--red" @click="removeItem(productLinkedIndex, customerIndex)">
                Remove
              </FormButton>
            </div>
            <div v-else>
              Nothing items linked fot this customer
            </div>
          </div>
          <form @submit.prevent="addItem(customerIndex)">
            <label for="items">Available items to add</label>
            <div class="customer__inputs">
              <FormSelect v-model="productId" name="items">
                <option v-for="(product, productIndex) in products" :key="productIndex" :value="productIndex">
                  {{ product.active ? product.name : null }}
                </option>
              </FormSelect>
              <FormButton class="formButton--green">
                Add item
              </FormButton>
            </div>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss">

.title {
  text-align: center;
}

.customer {

  &__instruction {
    font-size: 20px;
    margin: 50px 0px;
  }

  &__box {
    margin-bottom: 15px;
  }

  &__name {
    font-size: 20px;
    margin-bottom: 10px;
    cursor: pointer;
  }

  &__data {
    overflow: hidden;
    margin-top: 10px;
    border: solid 1px white;
    border-radius: 4px;
    height: 0vh;
    transition: height 2s ease;
  }

  &__itemsList {
    margin-bottom: 15px
  }

  &__item {
    display: flex;
    justify-content: space-between;
    border-bottom: dotted 1px white;
  }

  &__inputs {
    display: flex;
    justify-content: space-evenly;
    margin: 15px 0px 5px ;
  }
}

.showData {
  transition: 1s;
  padding: 12px 8px;
  height: auto;
  transition: height 2s ease;
}


</style>


