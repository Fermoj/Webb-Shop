<script setup>
import HomeComp from "@/components/HomeComp.vue";
</script>

<template>
  <v-app>
    <!-- Värdet för propen: getSearchTerm, computed håller koll på search för att se värdet som matas in -->
    <home-comp :search-value="getSearchTerm" />
  </v-app>
</template>

<script>
export default {
  props: { searchTerm: "" },
  components: {
    "home-comp": HomeComp
  },
  computed: {
    getSearchTerm() {
      return this.searchTerm;
    }
  }
};
</script>
