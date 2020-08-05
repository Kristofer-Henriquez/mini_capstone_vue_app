<template>
  <div class="home">
<!-- Here's where the create stuff starts -->

    <h1>{{ message }}</h1>
    <p>Click below to add a product to the database!</p>
    <p>Name: <input v-model="newProductName" type="text"></p>
    <p>Description: <input v-model="newProductDescription" type="text"></p>
    <p>Price: <input v-model="newProductPrice" type="text"></p>
    <p>Image_url: <input v-model="newProductImageUrl" type="text"></p>
    <button v-on:click="createProduct()"> Product Creation! </button>

<!-- And here's where the index stuff starts -->

    <h1> See All Products Below </h1>
    <div v-for="product in products">
      <p> name: {{ product.name }} </p>
      <p> description {{ product.description }} </p>
      <p> price: {{ product.price }} </p>
      <p> image: {{ product.image_url }} </p>
      <button v-on:click="showProduct(product)"> Show more info </button>
      <br>
      <br>
      <br>
      <br>
      
      <img v-bind:src="product.image_url">
      <hr>
    </div>

    <!-- Here's where the show info starts -->

    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info</h1>
        <p>Id: {{currentProduct.id}}</p>
        <p>Name: <input v-model="currentProduct.name"></p>
        <p>Description: <input v-model="currentProduct.description"></p>
        <p>Price: <input v-model="currentProduct.price"></p>
        <p>Image_url: <input v-model="currentProduct.image_url"></p>
        <button v-on:click="updateProduct(currentProduct)"> Update product! </button>
        <button v-on:click="destroyProduct(currentProduct)"> Delete product! </button>
        <button> Close </button>
      </form>
    </dialog>


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
      products: [],
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: "",
      currentProduct: [],
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
        name: this.newProductName,
        description: this.newProductDescription,
        price: this.newProductPrice,
        image_url: this.newProductImageUrl
      };
    
      axios.post("/api/products", params).then(response => {
        console.log(response.data);
        this.products.push(response.data);
      });
    },

    showProduct: function(product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function(product) {
      console.log(product);

      var params = {
        name: this.currentProduct.name,
        description: this.currentProduct.description,
        price: this.currentProduct.price,
        image_url: this.currentProduct.image_url
      };

      axios.patch("/api/products/" + this.currentProduct.id, params).then(response => {
        console.log(response.data);
        this.currentProduct = response.data;
      });
      

    },
    
    destroyProduct: function(product) {
      console.log(product);

      axios.delete("/api/products/" + this.currentProduct.id).then(response => {
        console.log(response.data);

        var index = this.products.indexOf(product);

        this.products.splice(index, 1);

        console.log(index);
      });
    }
  },
  
};
</script>