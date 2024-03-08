<script>
import axios from 'axios'
export default {

  data() {
    return {
      customers: [],
      products: []
    }
  },

  async beforeMount() {
    try {
      const responseCustomers = await axios.get('https://customer-products-test-default-rtdb.firebaseio.com/customer.json')
      const responseDataCustomers = responseCustomers.data
      this.customers = Object.keys(responseDataCustomers).map(key => responseDataCustomers[key]);

      const responseProducts = await axios.get('https://customer-products-test-default-rtdb.firebaseio.com/product.json')
      const responseDataProducts = responseProducts.data
      this.products = Object.keys(responseDataProducts).map(key => responseDataProducts[key])

    } catch (err) {
      console.error('Errro on GET request products: ', err)
    }
  }
}
</script>

<template>
  <h2>User and Products</h2>
  <div>
    <div v-for="customer in customers">
      <span >
        {{ customer.name }}
      </span>
    </div>
    <div v-for="product in products">
      <span >
        {{ product.name }}
      </span>
    </div>
    <div>Customer</div>
    <div>Products</div>
  </div>
</template>


