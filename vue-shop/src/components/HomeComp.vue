<template>
  <v-app class="mt-n10">
    <v-carousel
      cycle
      class="pr-10 pl-10 pt-4"
      height="650"
      hide-delimiter-background
      :show-arrows="false"
    >
      <v-carousel-item
        v-for="slide in slides.slice(18, 21)"
        :key="slide.id"
        class="hero-image"
      >
        <v-img :src="slide.heroImage" alt="strumpor, kampanj" />
        <v-container class="hero-text-btn">
          <v-card class="socks-name">
            <v-card-text>
              <div class="text-h4">
                {{ slide.name }}
              </div>
            </v-card-text>
          </v-card>
          <div class="btn">
            <v-btn @click="addToFavorites(product)">{{
              slide.btnAction
            }}</v-btn>
          </div>
        </v-container>
      </v-carousel-item>
    </v-carousel>

    <p class="hero-title ml-4 mb-5">Populärt just nu</p>
    <v-sheet elevation="8" class="mb-14">
      <v-slide-group v-model="model" class="mb-4 pa-4" show-arrows>
        <v-slide-group-item
          v-for="product in filteredProducts.slice(0, 18)"
          :key="product.id"
          v-slot="{ isSelected, toggle }"
        >
          <v-card class="card ma-6" @click="toggle" height="auto" width="300">
            <div class="container">
              <router-link :to="`product/${product.id}`">
                <v-img :src="product.LargeImage" class="ma-5" />
              </router-link>
              <div class="d-flex align-center justify-center">
                <v-scale-transition>
                  <v-icon
                    v-if="isSelected"
                    color="white"
                    size="60"
                    class="icons"
                  >
                    <v-btn variant="outlined"> {{ product.size[0] }} </v-btn>
                    <v-btn variant="outlined"> {{ product.size[1] }} </v-btn>
                    <v-btn
                      @click="addToCart(product)"
                      class="ma-5"
                      icon="mdi-cart"
                      variant="outlined"
                    ></v-btn>
                  </v-icon>
                </v-scale-transition>
              </div>
              <v-row justify="start">
                <v-card-item class="mt-5 ml-3 mb-5">
                  <h4>{{ product.name }}</h4>
                  <v-spacer></v-spacer>
                  <p>{{ product.price }} SEK</p></v-card-item
                ><v-spacer></v-spacer
                ><v-card-actions>
                  <v-btn
                    class="mr-5 mt-5 mb-5"
                    icon
                    small
                    @click="addToFavorites(product)"
                  >
                    <v-icon>{{
                      isFavorite(product) ? "mdi-heart" : "mdi-heart-outline"
                    }}</v-icon>
                  </v-btn>
                </v-card-actions>
              </v-row>
            </div>
          </v-card>
        </v-slide-group-item>
      </v-slide-group>
    </v-sheet>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
props: { searchValue: "" },

  data() {
    return {
      slides: [],
      model: null,
      products: [],
      cartItems: JSON.parse(localStorage.getItem("cartItems")) || [],
      favorites: JSON.parse(localStorage.getItem("favorites")) || [],
    };
  },
  mounted() {
    axios.get("/Product.json").then((response) => {
      this.slides = response.data;
      this.products = response.data;
    });
  },
  computed: {
    filteredProducts() {
      if (!this.searchValue) {
        return this.products;
      } else {
        //filter skapar en ny array  som evalueras till true i callbacken "product"
        const productsArray = this.products.filter(product => {
          const searchTerm = this.searchValue.toLowerCase().trim();
          const productName = product.name.toLowerCase();
          // om söktermen inkluderas i produktnamnet så evaluares det till true och renderar ut produkten.
          return productName.includes(searchTerm);
        });
        return productsArray;
      }
    }
  },
  methods: {
    addToFavorites(product) {
      const index = this.favorites.findIndex((item) => item.id === product.id);
      if (index === -1) {
        this.favorites.push(product);
      } else {
        this.favorites.splice(index, 1);
      }
      localStorage.setItem("favorites", JSON.stringify(this.favorites));
    },
    isFavorite(product) {
      return this.favorites.some((favorite) => favorite.id === product.id);
    },
    addToCart(product) {
      const QUANTITY = 1;
      const existingItem = this.cartItems.find(
        (item) => item.name === product.name && item.size === product.size
      );
      if (existingItem) {
        existingItem.quantity += QUANTITY;
      } else {
        this.cartItems.push({
          name: product.name,
          price: product.price,
          size: product.size,
          quantity: QUANTITY,
        });
      }
      localStorage.setItem("cartItems", JSON.stringify(this.cartItems));
    },
  },
};
</script>

<style scoped>
.container {
  height: 100%;
  position: relative;
}

.hero-title {
  font-size: 2rem;
  padding: 6px;
}
.hero-text-btn {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 0;
  left: 20%;
  width: 100%;
  height: 40%;
  padding: 20px;
}
.socks-name {
  display: flex;
  justify-content: center;
  align-items: center;

  box-shadow: 10px 5px 5px #000000;
  background-color: #87cefa;
}
.btn {
  position: relative;
  box-shadow: 5px 5px 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 1px;
}
</style>
