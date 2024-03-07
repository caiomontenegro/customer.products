<script>
import axios from 'axios'
export default {
  data() {
    return {
      name: '',
      active: false
    }
  },

  methods: {
    async submitProduct() {
      try {
        const response = await axios.post('https://customer-products-test-default-rtdb.firebaseio.com/register-product.json', {
          name: this.name,
          active: this.active
        })
      } catch (err) {
        console.error("Error on request POST:", err)
      }
    }
  }
}
</script>

<template>
  <form @submit.prevent="submitProduct">
    <label for="name">Product Name</label>
    <input type="text" v-model="name">

    <label for="active">Active</label>
    <input type="radio" v-model="active">
    <button type="submit">Submit</button>
  </form>
</template>