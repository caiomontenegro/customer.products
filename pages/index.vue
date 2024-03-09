<script>
import axios from 'axios'
export default {

  data() {
    return {
      customers: [],
      products: [],
      productsLinked: null,
      productId: null
    }
  },

  async beforeMount() {
    try {
      const responseCustomers = await axios.get('https://customer-products-test-default-rtdb.firebaseio.com/customer.json')
      this.customers = responseCustomers.data

      const responseProducts = await axios.get('https://customer-products-test-default-rtdb.firebaseio.com/product.json')
      this.products = responseProducts.data

    } catch (err) {
      console.error('Errro on GET request products: ', err)
    }
  },

  methods: {
    async addItem(customerId) {
      const currentProducts = await this.customers[customerId].products || {}
      const newProduct = await {[this.productId] : this.products[this.productId]}
      try {
        const updatedProducts = { ...currentProducts, ...newProduct }
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
  <h2>Customers Link Items</h2>
  <span>(Only actives)</span>
  <div>
    <div v-for="(customer, customerIndex) in customers">
      <div v-if="customer.active">
        <div v-for="(productLinked, productLinkedIndex) in customer.products" :key="productLinkedIndex">
          <span>{{ productLinked.name }}</span>
          <button @click="removeItem(productLinkedIndex, customerIndex)">Remove Item</button>
        </div>
        <form @submit.prevent="addItem(customerIndex)">
          <label for="items">Items enable to add (only actives)</label>
          <select v-model="productId" name="items">
            <option v-for="(product, productIndex) in products" :key="productIndex" :value="productIndex">
              {{ product.active ? product.name : null }}
            </option>
          </select>
          <button>Add Item</button>
        </form>
      </div>
    </div>
  </div>
</template>


