<template>
  <div>
    <h2 class="text-2xl font-bold mb-4">Movie List</h2>
    <div v-if="loading" class="text-gray-500">Loading movies...</div>
    <div v-if="error" class="text-red-500">{{ error }}</div>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 mt-4">
      <div
        v-for="movie in movies"
        :key="movie.id"
        class="bg-white rounded-lg shadow-lg p-4"
      >
        <h3 class="text-xl font-semibold">{{ movie.title }}</h3>
        <p class="text-sm text-gray-700">
          Release Date: {{ movie.release_date }}
        </p>
        <BarcodeUser :value="movie.id.toString()" class="mt-4" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import BarcodeUser from "./BarcodeUser.vue";

export default {
  name: "MovieList",
  components: {
    BarcodeUser,
  },
  mounted() {
    console.log(process.env.API_KEY);
  },
  props: {
    searchQuery: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      movies: [],
      loading: false,
      error: null,
    };
  },
  watch: {
    searchQuery: {
      immediate: true,
      handler(newQuery) {
        if (newQuery) {
          this.fetchMovies(newQuery); // Fetch movies on search query change
        }
      },
    },
  },
  methods: {
    async fetchMovies(query) {
      // Directly use the provided API key
      const apiKey = "7bd3f902e99719c5aa57356eb3acb07e"; // The API key provided

      // Constructing the URL for the movie search, allowing dynamic query
      const url = `https://api.themoviedb.org/3/search/movie?query=${encodeURIComponent(
        query
      )}&api_key=${apiKey}`;

      this.loading = true;
      this.error = null;

      try {
        const response = await axios.get(url, {
          headers: {
            accept: "application/json",
          },
        });

        // Update movies with fetched data
        this.movies = response.data.results;
      } catch (err) {
        this.error = "Error fetching data, please try again.";
        console.error(err.response ? err.response.data : err.message); // Log the error for debugging
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>
