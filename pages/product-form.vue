<script>
import axios from 'axios'
export default {
  data() {
    return {
      key: null,
      name: '',
      active: false,
      products: [],
      productForm: false,
      invalidForm: false
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
    checkfields() {
      if (this.name === '') {
        this.invalidForm = true
      } else {
        this.invalidForm = false
      }
    },

    async submitProduct() {
      if (this.key) {
        this.checkfields()
        if(this.invalidForm) {
          return
        } else {
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
        }
      } else {

        this.checkfields()
        if(this.invalidForm) {
          return
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
      this.name = ''
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
  <div class="title">
    <h2>Products List</h2>
  </div>
  <div >
    <div>
      <div class="productsData" v-for="(product, index) in products" key="products">
        <div>
          {{ product.name }}
          {{ product.active ? "active" : "inactive" }}
        </div>
        <div>
          <FormButton @click="editProduct(product.name, product.active, index)">Edit</FormButton>
          <FormButton class="formButton--red" @click="deleteProduct(index)">Delete</FormButton>
        </div>
      </div>
      <div class="options">
        <FormButton class="formButton--blue" @click="registerProduct">Register New Product</FormButton>
      </div>
    </div>
  </div>
  <div v-if="productForm">
    <form class="form" @submit.prevent="submitProduct">
      <div class="title">
        <h3>Products Form</h3>
      </div>
      <div class="form__input">
        <label for="name">Product Name</label>
        <input placeholder="name" type="text" v-model="name">
      </div>
      <div>
        <label for="active">Active</label>
        <input placeholder="active" type="checkbox" v-model="active">
      </div>
      <div>
        <i :class="{form__alert : true , show : invalidForm}">
          Product field are mandatory!
        </i>
      </div>
      <div class="form__actions">
        <FormButton class="formButton--blue" type="submit">Submit</FormButton>
        <FormButton class="formButton--red" @click="closeProductForm">Cancel</FormButton>
      </div>
    </form>
  </div>
</template>

<style lang="scss" scoped>

.productsData {
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
    font-weight: bolder;
  }

  .show {
    display: block;
  }
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