<script>
import axios from 'axios'
export default {
  data() {
    return {
      customers: [],
      key: null,
      name: '',
      phone: '',
      document: '',
      email: '',
      active: false,
      customerForm: false,
      invalidForm: false
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
    checkfields() {
      if(this.name === '' || this.phone === '' || this.document === '' || this.email === '') {
        console.log('campos em branco')
        this.invalidForm = true
      } else {
        // console.log('campos preenchidos')
        // console.log('name ', this.name, 'phone ', this.phone, 'document ', this.document, 'email ', this.email)
        this.invalidForm = false
      }
    },

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
          })
          if(responsePatchCustomer.status === 200) {
            location.reload()
          }
        } catch(err) {
          console.error("Error to update the customer: ", error)
        }

      } else {
        this.checkfields()
        if(this.invalidForm) {
          console.log(this.invalidForm)
          return
        } else {
          console.log(this.invalidForm)
          try {
            const responsePostCustomer = await axios.post('https://customer-products-test-default-rtdb.firebaseio.com/customer.json',
            {
              name: this.name,
              phone: this.phone,
              document: this.document,
              email: this.email,
              active: this.active,
              products: [],
            })
            if(responsePostCustomer.status = 200) {
              // location.reload()
            }
          } catch (err) {
            console.error('Error on POST request:', err)
          }
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
      this.name = ''
      this.active = false
      this.document = ''
      this.email = ''
      this.phone = ''
    },

    closeCustomerForm() {
      this.customerForm = false
    }
  }
}
</script>

<template>
  <div class="title">
    <h2>Users List</h2>
  </div>
  <div>
    <div>
      <div class="customerData" v-for="(customer, index) in customers" key="customer">
        <div>
          {{ customer.name }}
          ({{ customer.active ? "active" : "inactivated" }})
        </div>
        <div>
          <FormButton @click="editCustomer(customer.name, customer.active, customer.document, customer.email, customer.phone, index)">Edit</FormButton>
          <FormButton class="formButton--red" @click="deleteCustomer(index)">Delete</FormButton>
        </div>
      </div>
      <div class="options">
        <FormButton class="formButton--blue" @click="registerCustomer()">Register New User</FormButton>
      </div>
    </div>
  </div>

  <div v-if="customerForm">
    <form class="form" @submit.prevent="submitCustomer">
      <div class="title">
        <h3>Customer Form</h3>
      </div>
      <div class="form__input">
        <label for="name">Customer Name</label>
        <input name="name" placeholder="name" type="text" v-model="name">
      </div>
      <div class="form__input">
        <label for="number">Phone Number</label>
        <input name="number" placeholder="phone" type="text" v-model="phone">
      </div>
      <div class="form__input">
        <label for="document">Document Number</label>
        <input name="document" placeholder="document" type="text" v-model="document">
      </div>
      <div class="form__input">
        <label for="email">E-mail</label>
        <input name="email" placeholder="email" type="text" v-model="email">
      </div>
      <div>
        <label for="active">Active</label>
        <input name="active" placeholder="active" type="checkbox" v-model="active">
      </div>
      <div>
        <i :class="{form__alert : true , show : invalidForm}">
          All of fields, except "Active" field are mandatory!
        </i>
      </div>
      <div class="form__actions">
        <FormButton class="formButton--blue" type="submit">Submit</FormButton>
        <FormButton class="formButton--red" @click="closeCustomerForm">Cancel</FormButton>
      </div>
    </form>
  </div>
</template>

<style lang="scss" scoped>

.customerData {
  display: flex;
  justify-content: space-between;
  border-bottom: dotted 1px white;
}

.options {
  display: flex;
  justify-content: center;
  margin: 30px 0px;
}

.form {

  border: solid 1px white;
  padding: 6px;
  border-radius: 4px;

  &__input {
    display: flex;
    justify-content: space-between;
  }

  &__actions {
    display: flex;
    justify-content: end;
    margin-top: 15px;
  }

  &__alert {
    display: none;
    color: red;
  }
}

.show {
  display: block;
}

@media(min-width: 1000px) {
  .form {
    padding: 30px 50px;
    &__input {
      justify-content: space-between;
    }
  }
}

</style>