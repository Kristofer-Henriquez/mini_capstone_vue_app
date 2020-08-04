<template>
  <div class="home">
<!-- Here's where the index stuff starts -->

    <h1>{{ message }}</h1>
    <p>Click below to add a product to the database!</p>
    <button v-on:click="createProduct()"> Product Creation! </button>

<!-- And here's where the create stuff starts -->

    <h1> See All Products Below </h1>
    <div v-for="product in products">
      <p> name: {{ product.name }} </p>
      <p> description {{ product.description }} </p>
      <p> price: {{ product.price }} </p>
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

    },
    createProduct: function() {
      console.log("This will create a product..");

      var params = {
        name: "Banana",
        description: "It's... a banana",
        price: 6,
        image_url: "https://media.makeameme.org/created/im-a-banana-5cb0a4.jpg"
      };
    
      axios.post("/api/products", params).then(response => {
        console.log(response.data);
        this.products.push(response.data);
      });
    }
  },
  
};
</script>