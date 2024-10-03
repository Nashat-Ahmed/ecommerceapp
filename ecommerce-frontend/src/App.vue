<template>
  <div id="app">
    <h1>Product Catalog</h1>
    <ul>
      <li v-for="product in products" :key="product.id">
        {{ product.name }} - ${{ product.price }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: []
    };
  },
  mounted() {
    fetch('http://localhost:5000/products')
      .then(response => response.json())
      .then(data => {
        this.products = data;
      });
  }
};
</script>

