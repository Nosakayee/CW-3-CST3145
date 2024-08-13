<template>
    <div class="container mt-4 container-custom" v-if="cart.length">
      <div class="row">
        <div class="col-sm-8 border rounded-3 p-3">
          <div v-for="item in cart" :key="item.lesson.id" class="border-bottom mb-3 pb-3">
            <div class="row">
              <div class="col-sm-4 text-center">
                <i :class="item.lesson.icon" class="display-1"></i>
              </div>
              <div class="col-sm-4">
                <p class="mb-1">Title: {{ item.lesson.title }}</p>
                <p class="mb-1">Quantity: {{ item.amount }}</p>
                <button class="btn btn-outline-danger btn-sm" @click='removeProduct(item.lesson)'>
                  <i class="fa fa-minus-circle"></i>
                </button>
              </div>
              <div class="col-sm-4">
                <p class="mb-1">Price: {{ item.lesson.price }} <i class="fa fa-eur"> per hour</i></p>
              </div>
            </div>
          </div>
          <div class="col-sm-12">
            <p>Total Price: {{ cartTotal }} <i class="fa fa-eur"></i></p>
          </div>
        </div>
        <div class="col-sm-4 mt-4">
          <div class="container">
            <div class="row justify-content-center">
              <div class="col-md-12">
                <div class="card">
                  <div class="card-body">
                    <h5 class="card-title">Details</h5>
                    <form @submit.prevent="submitOrder">
                      <div class="mb-3">
                        <label for="username" class="form-label">Name</label>
                        <input type="text" v-model="username" class="form-control" id="username" name="username" placeholder="Name" required>
                      </div>
                      <div class="mb-3">
                        <label for="phone" class="form-label">Number</label>
                        <input type="number" v-model="phone" class="form-control" id="phone" name="phone" placeholder="Number" required>
                      </div>
                      <button type="submit" class="btn btn-primary" :disabled="!completeOrder">
                        Submit
                      </button>
                    </form>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <p>Your cart is empty.</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'CheckoutList',
    props: ['cart'],
    data() {
      return {
        username: '',
        phone: '',
      };
    },
    computed: {
      cartTotal() {
        return this.cart.reduce((total, item) => total + item.lesson.price * item.amount, 0);
      },
      completeOrder() {
        return this.username && this.phone && this.cart.length > 0;
      }
    },
    methods: {
      removeProduct(lesson) {
        this.$emit('remove-item-from-cart', lesson);
      },
      submitOrder() {
        alert('Order submitted!');
      }
    }
  };
  </script>
  
  <style scoped>
  .container-custom {
    padding-top: 20px;
  }
  .display-1 {
    font-size: 4rem;
  }
  </style>
  