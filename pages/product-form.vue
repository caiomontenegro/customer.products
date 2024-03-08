<script>
import axios from 'axios'
export default {
  data() {
    return {
      name: '',
      active: false,
      products: [],
      registerForm: false
    }
  },

  async beforeMount() {
    try {
      const responseProducts = await axios.get('https://customer-products-test-default-rtdb.firebaseio.com/product.json')
      this.products = responseProducts.data
    } catch (err) {
      console.error('Error to load the products: ', err)
    }
  },


  methods: {
    async submitProduct() {
      try {
        const response = await axios.post('https://customer-products-test-default-rtdb.firebaseio.com/product.json', {
          name: this.name,
          active: this.active
        })
        if(response.status = 200) {
          location.reload()
        }
      } catch (err) {
        console.error("Error on request POST:", err)
      }
    }, 

    async deleteProduct(id) {
      try {
        const responseDeleteProduct = await axios.delete(`https://customer-products-test-default-rtdb.firebaseio.com/product/${id}.json`)
        if (responseDeleteProduct.status === 200) {
          location.reload()
        }
      } catch(err) {
        console.error("Error to delete the product: ", err)
      }
    },

    openRegisterForm() {
      this.registerForm = true
    }
  }
}
</script>

<template>
  <div >
    <div>
      <h2>Products List</h2>
      <div v-for="(product, index) in products" key="products">
        {{ product.name }}
        {{ product.active ? "active" : "inactive" }}
        <button>Edit</button>
        <button @click="deleteProduct(index)">Delete</button>
      </div>
      <button @click="openRegisterForm">Register New Product</button>
    </div>
  </div>
  <div v-if="registerForm">
    <form @submit.prevent="submitProduct">
      <label for="name">Product Name</label>
      <input type="text" v-model="name">
      <label for="active">Active</label>
      <input type="radio" v-model="active">
      <button type="submit">Submit</button>
    </form>
  </div>
</template>