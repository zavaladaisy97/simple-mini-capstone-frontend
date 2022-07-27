<script>
import axios from "axios";

export default {
  data: function () {
    return {
      products: [],
      errors: [],
      message: "Welcome",
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products.json").then((response) => {
        this.products = response.data;
        console.log("All products: ", response.data);
      });
    },
    createProduct: function () {
      console.log("new product created");

      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        image_url: this.newImageUrl,
      };

      axios
        .post("http://localhost:3000/products.json", params)
        .then((response) => {
          console.log("Success!", response.data);
          this.products.push(response.data);
          this.newProductName = "";
          this.newProductPrice = "";
          this.newProductDescription = "";
          this.newImageUrl = "";
        })
        .catch((error) => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
    showProduct: function (product) {
      console.log(product);
      document.querySelector("#product-info").showModal();
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>NewProduct</h1>
    Name:
    <input type="text" v-model="newProductName" />
    Price:
    <input type="number" v-model="newProductPrice" />
    Description:
    <input type="text" v-model="newProductDescription" />
    ImageUrl:
    <input type="text" v-model="newProductImageUrl" />
    <button v-on:click="createProduct()">Create Product</button>
    <div class="errors" v-for="error in errors" v-bind:key="error">
      {{ error }}
    </div>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>Title: {{ product.name }}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.title" />
      <p>Price: {{ product.price }}</p>
      <button v-on:click="showProduct()">More info</button>
    </div>

    <dialog id="product-info">
      <form method="dialog">
        <h1>Product Info:</h1>
        <p>Name:</p>
        <p>Price:</p>
        <p>Description:</p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
.home {
  color: pink;
}
img {
  max-width: 500px;
}
.errors {
}
</style>
