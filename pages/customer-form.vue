<script>
import axios from 'axios'
export default {
  data() {
    return {
      customers: [],
      key: null,
      name: null,
      phone: null,
      document: null,
      email: null,
      active: false,
      customerForm: false
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
      if(this.key) {
        
        try {
          const responsePatchCustomer = await axios.patch(`https://customer-products-test-default-rtdb.firebaseio.com/customer/${this.key}.json`,
          {
            name: this.name,
            phone: this.phone,
            document: this.document,
            email: this.email,
            active: this.active,
            products: []
          })
          if(responsePatchCustomer.status === 200) {
            location.reload()
          }
        } catch(err) {
          console.error("Error to update the customer: ", error)
        }

      } else {
        try {
          const responsePostCustomer = await axios.post('https://customer-products-test-default-rtdb.firebaseio.com/customer.json',
          {
            name: this.name,
            phone: this.phone,
            document: this.document,
            email: this.email,
            active: this.active,
            products: []
          })
          if(responsePostCustomer.status = 200) {
            location.reload()
          }
        } catch (err) {
          console.error('Error on POST request:', err)
        }
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

    editCustomer(name, status, document, email, phone, index) {
      this.customerForm = true
      this.key = index
      this.name = name
      this.active = status
      this.document = document
      this.email = email
      this.phone = phone
    },

    registerCustomer () {
      this.customerForm = true
      this.key = null
      this.name = null
      this.active = false
      this.document = null
      this.email = null
      this.phone = null
    },

    closeCustomerForm() {
      this.customerForm = false
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
        <button @click="editCustomer(customer.name, customer.active, customer.document, customer.email, customer.phone, index)">Edit</button>
        <button @click="deleteCustomer(index)">Delete</button>
      </div>
      <button @click="registerCustomer()">Register New User</button>
    </div>
  </div>

  <div v-if="customerForm">
    <form @submit.prevent="submitCustomer">
      <label for="name">Customer Name</label>
      <input placeholder="name" type="text" v-model="name">
      <label for="number">Phone Number</label>
      <input placeholder="phone" type="text" v-model="phone">
      <label for="document">Document Number</label>
      <input placeholder="document" type="text" v-model="document">
      <label for="email">E-mail</label>
      <input placeholder="email" type="text" v-model="email">
      <label for="active">Active</label>
      <input placeholder="active" type="checkbox" v-model="active">
      <button type="submit">Submit</button>
      <button @click="closeCustomerForm">Cancel</button>
    </form>
  </div>
</template>