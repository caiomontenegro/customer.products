<script>
import axios from 'axios'
export default {
  data() {
    return {
      name: '',
      phone: '',
      document: '',
      email: '',
      active: false
    }
  },

  methods: {
    async submitCustomer() {
      try {
        const response = await axios.post('https://customer-products-test-default-rtdb.firebaseio.com/register-customer.json', 
        {
          name: this.name,
          phone: this.phone,
          document: this.document,
          email: this.email,
          active: this.active
        })
      } catch (err) {
        console.error('Error on POST request:', err)
      }
    }
  }
}
</script>

<template>
  <form @submit.prevent="submitCustomer">
    <label for="name">Customer Name</label>
    <input type="text" v-model="name">

    <label for="number">Phone Number</label>
    <input type="text" v-model="phone">

    <label for="document">Document Number</label>
    <input type="text" v-model="document">

    <label for="email">E-mail</label>
    <input type="text" v-model="email">

    <label for="active">Active</label>
    <input type="checkbox" v-model="active">

    <button type="submit">Submit</button>
  </form>
</template>