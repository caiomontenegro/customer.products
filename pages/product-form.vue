<script>
import axios from 'axios'
export default {
  data() {
    return {
      key: null,
      name: null,
      active: false,
      products: [],
      productForm: false
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
      if (this.key) {
        try {
          const responseEditProduct = await axios.patch(`https://customer-products-test-default-rtdb.firebaseio.com/product/${this.key}.json`, {
            name: this.name,
            active: this.active
          })
          if (responseEditProduct.status === 200) {
            location.reload()
          }
        } catch(err) {
          console.error("Error to update the product: ", err)
        }
      } else {
        try {
          const responsePostProduct = await axios.post('https://customer-products-test-default-rtdb.firebaseio.com/product.json', {
            name: this.name,
            active: this.active
          })
          if(responsePostProduct.status = 200) {
            location.reload()
          }
        } catch (err) {
          console.error("Error on request POST:", err)
        }
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

    editProduct(name, status, index) {
      this.name= name
      console.log(status)
      this.active = status
      this.key = index
      this.productForm = true
    },

    registerProduct() {
      this.name= null
      this.active = null
      this.key = null
      this.productForm = true
    },

    closeProductForm() {
      this.productForm = false
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
        <button @click="editProduct(product.name, product.active, index)">Edit</button>
        <button @click="deleteProduct(index)">Delete</button>
      </div>
      <button @click="registerProduct">Register New Product</button>
    </div>
  </div>
  <div v-if="productForm">
    <form @submit.prevent="submitProduct">
      <label for="name">Product Name</label>
      <input placeholder="name" type="text" v-model="name">
      <label for="active">Active</label>
      <input placeholder="active" type="checkbox" v-model="active">
      <button type="submit">Submit</button>
      <button @click="closeProductForm">Cancel</button>
    </form>
  </div>
</template>