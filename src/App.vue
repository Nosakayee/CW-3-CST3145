<template>
  <div id="app">
    <button @click="toggleView">
      {{ cartSize }} <i class="fa fa-cart-plus"></i>
    </button>

    <component :is="currentView" :lessons="lessons" :cart="cart" @add-item-to-cart="addToCart"
      @remove-item-from-cart="removeFromCart"></component>
  </div>
</template>

<script>
import Lesson from './components/Lesson.vue';
import Checkout from './components/Checkout.vue';

export default {
  name: 'App',
  components: {
    Lesson,
    Checkout,
  },
  data() {
    return {
      currentView: 'Lesson',
      lessons: [],
      cart: [],
    };
  },
  created() {
    this.fetchLessons();
  },
  methods: {
    toggleView() {
      this.currentView = this.currentView === 'Lesson' ? 'Checkout' : 'Lesson';
    },
    async fetchLessons() {
      const response = await fetch('https://cst-3145-coursework-2.vercel.app/collection/lessons');
      this.lessons = await response.json();
    },
    addToCart(lesson) {
      const cartItem = this.cart.find(item => item.lesson.id === lesson.id);
      if (cartItem) {
        cartItem.amount++;
      } else {
        this.cart.push({ lesson, amount: 1 });
      }
      lesson.availability--;
    },
    removeFromCart(lesson) {
      const index = this.cart.findIndex(item => item.lesson.id === lesson.id);
      if (index !== -1) {
        this.cart[index].amount--;
        if (this.cart[index].amount === 0) {
          this.cart.splice(index, 1);
        }
        lesson.availability++;
      }
    },
  },
  computed: {
    cartSize() {
      return this.cart.reduce((sum, item) => sum + item.amount, 0);
    },
  },
};
</script>

<style>
</style>