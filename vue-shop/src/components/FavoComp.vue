<template>
    <v-container>
      <h1>Favorites</h1>
      <v-row>
        <v-col
          v-for="favorite in favorites"
          :key="favorite.id"
          cols="6"
          sm="4"
          md="3"
        >
          <v-card>
            <v-img :src="favorite.LargeImage"></v-img>
            <v-card-title>{{ favorite.name }}</v-card-title>
            <v-card-actions>
              <v-btn icon small @click="addToFavorites(favorite)">
                <v-icon>{{
                  isFavorite(favorite) ? "mdi-heart" : "mdi-heart-outline"
                }}</v-icon>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>

  <script>
  import axios from "axios";

  export default {
    data() {
      return {
        products: [],
        favorites: JSON.parse(localStorage.getItem("favorites") || "[]"),
      };
    },
    mounted() {
      axios.get("/Product.json").then((response) => {
        this.products = response.data;
      });
    },
    methods: {
      addToFavorites(product) {
        let index = this.favorites.findIndex((item) => item.id === product.id);
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
    },
  };
  </script>
