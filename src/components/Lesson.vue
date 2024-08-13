<template>
    <div>
      <div class="mb-3">
        <input type="text" class="form-control" v-model="searchTerm" placeholder="Search Product..." @input="searchLessons">
      </div>
      <div class="mb-3">
        <label for="sortAttribute">Sort By:</label>
        <select v-model="sortAttribute" class="form-select">
          <option value="title">Title</option>
          <option value="subject">Subject</option>
          <option value="location">Location</option>
          <option value="price">Price</option>
          <option value="availability">Available</option>
        </select>
      </div>
  
      <div class="mb-3">
        <label for="sortOrder">Sort Order:</label>
        <select v-model="sortOrder" class="form-select">
          <option value="TopToBottom">Ascending</option>
          <option value="BottomToTop">Descending</option>
        </select>
      </div>
  
      <div class="row">
        <div class="col-sm-4 mb-3" v-for="product in filteredAndSortedLessons" :key="product.id">
          <div class="card h-100 shadow-sm">
            <div class="card-header d-flex">
              <div>
                <p class="mb-1">Subject: {{ product.subject }}</p>
                <h5 class="mb-0">Title: {{ product.title }}</h5>
              </div>
              <i :class="product.icon" class="fs-1 ms-auto"></i>
            </div>
            <div class="card-body">
              <p class="mb-1">Price: {{ product.price }} <i class="fa fa-eur"> per hour</i></p>
              <p class="mb-1">Location: {{ product.location }}</p>
              <p class="mb-1">Available: {{ product.avaliability }}</p>
            </div>
            <div class="card-footer d-flex justify-content-between">
              <button class="btn btn-outline-success" @click="addToCart(product)" :disabled="product.avaliability === 0">
                <i class="fa fa-cart-plus"></i>
              </button>
              <button class="btn btn-outline-danger" @click="removeProduct(product)">
                <i class="fa fa-minus-circle"></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'LessonList',
    props: ['lessons'],
    data() {
      return {
        searchTerm: '',
        sortAttribute: 'title',
        sortOrder: 'TopToBottom',
      };
    },
    computed: {
      filteredAndSortedLessons() {
        let filteredLessons = this.lessons.filter(lesson => {
          return (
            lesson.title.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
            lesson.subject.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
            lesson.location.toLowerCase().includes(this.searchTerm.toLowerCase())
          );
        });
  
        return filteredLessons.sort((a, b) => {
          let comparison = 0;
          if (this.sortAttribute === 'price' || this.sortAttribute === 'availability') {
            comparison = a[this.sortAttribute] - b[this.sortAttribute];
          } else {
            comparison = a[this.sortAttribute].localeCompare(b[this.sortAttribute]);
          }
          return this.sortOrder === 'TopToBottom' ? comparison : -comparison;
        });
      }
    },
    methods: {
      searchLessons() {
        // This method is intentionally left blank as filtering is handled in the computed property
      },
      addToCart(product) {
        this.$emit('add-item-to-cart', product);
      },
      removeProduct(product) {
        this.$emit('remove-item-from-cart', product);
      }
    }
  };
  </script>
  
  <style scoped>
  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  </style>
  