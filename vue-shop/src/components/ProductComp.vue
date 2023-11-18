<template>
  <v-container>
    <v-row>
      <v-col cols="12" md="7">
        <v-img
          v-for="product in getData($route.params.id)"
          :key="product.id"
          :src="product.LargeImage"
        ></v-img>
      </v-col>
      <v-col cols="12" md="5">
        <v-card>
          <v-card-title
            class="text-h4"
            v-for="product in getData($route.params.id)"
            :key="product.id"
          >
            {{ product.name }}
          </v-card-title>
          <v-card-subtitle
            v-for="product in getData($route.params.id)"
            :key="product.id"
          >
            <div>{{ product.price }} SEK</div>
            <v-rating class="ml-2" :value="3" :readonly="true"></v-rating>
          </v-card-subtitle>
          <v-text-field
            v-model="quantity"
            type="number"
            min="0"
            label="Quantity"
            outlined
          ></v-text-field>
          <v-select
            label="Size"
            :items="sizes"
            outlined
            item-text="size"
          ></v-select>
          <div v-for="product in getData($route.params.id)" :key="product.id">
            <v-btn
              color="#026CAD"
              class="mt-4 mx-16"
              @click="addToCart(product, quantity)"
              >ADD TO BASKET</v-btn
            >
            <v-btn
              class="mr-5 mt-5 mb-5"
              icon
              color="#F6A8B6"
              small
              @click="addToFavorites(product)"
            >
              <v-icon>{{
                isFavorite(product) ? "mdi-heart" : "mdi-heart-outline"
              }}</v-icon>
            </v-btn>
          </div>
          <v-card>
            <v-card-text>
              <div class="text-subtitle-1">
                Free deliveries to Sweden within 1-3 working days. <br />
                * Free climate-compensated deliveries
              </div>
            </v-card-text>
          </v-card>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" class="pb"> <span> Productdescription</span> </v-col>
    </v-row>
    <v-row v-for="product in getData($route.params.id)" :key="product.id">
      <v-col cols="12" class="pd" md="8" lg="6">
        <h3 class="font-weight-bold">{{ product.name }}</h3>
        <v-card-text>{{ product.info }}</v-card-text>
      </v-col>
    </v-row>
  </v-container></template
>
<script>
import axios from "axios";
export default {
  data() {
    return {
      products: [],
      items: [],
      quantity: 0,
      sizes: [],
      name: "",
      price: "",
      cartItems: [],
      favorites: JSON.parse(localStorage.getItem("favorites") || "[]")
    };
  },
  methods: {
    fetchData() {
      axios
        .get("/Product.json")
        .then(response => {
          this.products = response.data;
          const selectedProduct = this.getData(this.$route.params.id)[0];
          this.sizes = selectedProduct.size;
        })
        .catch(error => {
          console.log(error);
        });
      if (localStorage.getItem("cartItems")) {
        this.cartItems = JSON.parse(localStorage.getItem("cartItems"));
      }
    },
    getData(id) {
      let data = this.products;
      return data.filter(item => {
        return item.id == id;
      });
    },
    addToFavorites(product) {
      let index = this.favorites.findIndex(item => item.id === product.id);
      if (index === -1) {
        this.favorites.push(product);
      } else {
        this.favorites.splice(index, 1);
      }
      localStorage.setItem("favorites", JSON.stringify(this.favorites));
    },
    isFavorite(product) {
      return this.favorites.some(favorite => favorite.id === product.id);
    },
    addToCart(product) {
      const existingItem = this.cartItems.find(
        item => item.name === product.name
      );
      if (existingItem) {
        existingItem.quantity++;
      } else {
        this.cartItems.push({
          name: product.name,
          price: product.price,
          sizes: product.size,
          quantity: 1
        });
      }
      localStorage.setItem("cartItems", JSON.stringify(this.cartItems));
    }
  },
  created() {
    this.fetchData();
  }
};
</script>
