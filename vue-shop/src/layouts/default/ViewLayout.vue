<script setup>
import SearchComp from "@/components/SearchComp.vue";
</script>

<template>
  <v-responsive class="d-flex align-center">
    <!-- Detta är navbaren som är synlig för desktop -->

    <SearchComp @search-value="getData" />
    <!-- navbaren som är synlig för desktop tar slut här -->
    <!-- Detta är det som finns innuti menyn som kommer ut från vänster  -->
    <v-app id="inspire">
      <v-navigation-drawer v-model="drawer" disable-resize-watcher>
        <v-list nav>
          <v-list-item
            :to="{ name: 'Home' }"
            prepend-icon="mdi-home"
            title="Hem"
            value="home"
            exact
          >
          </v-list-item>
          <v-list-item
            :to="{ name: 'Dam' }"
            prepend-icon="mdi-gender-female"
            title="Dam"
            value="dam"
            exact
          >
          </v-list-item>
          <v-list-item
            :to="{ name: 'Herr' }"
            prepend-icon="mdi-gender-male"
            title="Herr"
            value="herr"
            exact
          >
          </v-list-item>
          <v-list-item
            :to="{ name: 'Barn' }"
            prepend-icon="mdi-teddy-bear"
            title="Barn"
            value="barn"
            exact
          >
          </v-list-item>

          <v-list-item
            :to="{ name: 'Product' }"
            prepend-icon="mdi-account-box-outline"
            title="Product"
            value="product"
            exact
          >
          </v-list-item>
          <v-list-item
            :to="{ name: 'Varukorg' }"
            prepend-icon="mdi mdi-cart-outline"
            title="Varukorg"
            value="varukorg"
          >
          </v-list-item>
          <v-list-item
            :to="{ name: 'favo' }"
            prepend-icon="mdi-heart"
            title="Favoriter"
            value="favo"
            exact
          ></v-list-item>
          <v-btn @click="toggleTheme" icon size="small">
            <v-icon>mdi-theme-light-dark</v-icon>
          </v-btn>
        </v-list>
      </v-navigation-drawer>
      <!-- menyn som kommer ut från vänster tar slut här -->
      <!-- Detta är hamburgarmenyn som syns när det är små skärmar -->
      <v-app-bar class="hidden-md-and-up">
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
        <v-toolbar-title>Vue Shop</v-toolbar-title>
        <!-- SÖKFÄLT -->
        <v-text-field
          @focus="searchClosed = false"
          @blur="searchClosed = true"
          v-model="searchTerm"
          @input="getData()"
          placeholder="Sök"
          variant="plain"
          prepend-inner-icon="mdi-magnify mt-auto w-25"
          :class="{ closed: searchClosed }"
        ></v-text-field>
        <v-btn icon :to="{ name: 'favo' }" title="Favoriter" value="favo" exact>
          <v-icon>mdi-heart</v-icon>
        </v-btn>
        <v-btn
          icon
          :to="{ name: 'Varukorg' }"
          title="Varukorg"
          value="varukorg"
          exact
        >
          <v-icon>mdi-cart</v-icon>
        </v-btn>
      </v-app-bar>
      <!-- hamburgarmenyn som syns när det är små skärmar tar sut här -->
      <!-- Här är all content som renderas på sidan mellan menyn och footern dvs BODY -->
      <v-main>
        <!-- Search ska färdas till alla routes -->
        <router-view :search-term="searchTerm" />
      </v-main>
      <!-- ----------------- RÖR EJ OVAN SEKTION ---------------------------- -->
      <v-footer>
        <v-row justify="center" no-gutters>
          <v-btn
            :href="`${link.route}`"
            v-for="link in links"
            :key="link"
            variant="text"
            class="mx-2"
            rounded="xl"
          >
            {{ link.name }}
          </v-btn>
          <v-col class="text-center mt-4" cols="12">
            {{ new Date().getFullYear() }} —
            <strong>SWEDISH VUESHOP AB</strong>
          </v-col>
        </v-row>
      </v-footer>
    </v-app>
  </v-responsive>
</template>
<script>
//import { useTheme } from "vuetify";

export default {
  data: () => ({
    searchTerm: "",
    searchClosed: true,
    drawer: false,
    links: [
      { name: "hem", route: "/" },
      { name: "dam", route: "/dam" },
      { name: "herr", route: "/herr" },
      { name: "barn", route: "/barn" }
    ]
  }),
  methods: {
    getData(searchTerm) {
      this.searchTerm = searchTerm;
    }
  }
};
</script>
<style scoped>
.closed {
  max-width: 25px;
}
</style>
