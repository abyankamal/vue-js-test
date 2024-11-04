<template>
  <div id="app" class="p-4">
    <MovieSearch @search="handleSearch" />
    <MovieList :searchQuery="searchQuery" />
  </div>
</template>

<script>
import MovieSearch from "./components/MovieSearch.vue";
import MovieList from "./components/MovieList.vue";

export default {
  name: "App",
  components: {
    MovieSearch,
    MovieList,
  },
  data() {
    return {
      searchQuery: "", // Shared state for the search query
    };
  },
  methods: {
    handleSearch(query) {
      this.searchQuery = query; // Update searchQuery when the search is triggered
    },
  },
};
</script>
