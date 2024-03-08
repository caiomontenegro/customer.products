<script>
import axios from 'axios'
export default {
  data() {
    return {
      customers: [],
      name: '',
      phone: '',
      document: '',
      email: '',
      active: false,
      registerForm: false
    }
  },

  async beforeMount() {
    try {
      const responseCustomers = await axios.get('https://customer-products-test-default-rtdb.firebaseio.com/customer.json')
      this.customers = responseCustomers.data
    } catch (err) {
      console.error("Error on loading the customers: ", err)
    }
  },

  methods: {
    async submitCustomer() {
      try {
        const response = await axios.post('https://customer-products-test-default-rtdb.firebaseio.com/customer.json',
        {
          name: this.name,
          phone: this.phone,
          document: this.document,
          email: this.email,
          active: this.active,
          products: []
        })
        if(response.status = 200) {
          location.reload()
        }
      } catch (err) {
        console.error('Error on POST request:', err)
      }
    },

    async deleteCustomer(id) {
      try {
        const responseDeleteCustomer = await axios.delete(`https://customer-products-test-default-rtdb.firebaseio.com/customer/${id}.json`)
        if (responseDeleteCustomer.status === 200) {
          location.reload()
        }
      } catch (err) {
        console.error("Error to delete user :", err)
      }
    },

    openRegisterForm () {
      this.registerForm = true
    }
  }
}
</script>

<template>
  <div>
    <div>
      <h2>Users List</h2>
      <div v-for="(customer, index) in customers" key="customer">
        {{ customer.name }}
        {{ customer.active ? "active" : "inactivated" }}
        <button>Edit</button>
        <button @click="deleteCustomer(index)">Delete</button>
      </div>
      <button @click="openRegisterForm">Register New User</button>
    </div>
  </div>

  <div v-if="registerForm">
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
  </div>
</template>