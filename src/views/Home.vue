<template>
  <div class="home">

    <h1>{{ message }}</h1>
    <div v-for="product in products">
      <p> name: {{ product.name }} </p>
      <p> description {{ product.description }} </p>
      <p> image: {{ product.image_url }} </p>
      <img v-bind:src="product.image_url">
      <hr>
    </div>

  </div>
</template>
<style>
</style>
<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: []
    };
  },
  created: function() {
    this.indexProducts();
  },
  methods: {
    indexProducts: function() {
      console.log("This will be the index...");

      axios.get("/api/products").then(response => {
        console.log(response);
        this.products = response.data;
      });

    }
  }
};
</script>