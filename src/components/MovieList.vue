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

        <!-- Image Rendering -->
        <img
          :src="`https://image.tmdb.org/t/p/w500${movie.backdrop_path}`"
          alt="Movie backdrop"
          class="w-full h-auto rounded-lg mb-4"
          v-if="movie.backdrop_path"
        />
        <!-- Show image if backdrop_path exists -->

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
      const apiKey = process.env.VUE_APP_API_KEY; // Load API key from environment variable
      const bearerToken = process.env.VUE_APP_BEARER_TOKEN; // Load Bearer token from environment variable

      // Constructing the URL for the movie search
      const url = `https://api.themoviedb.org/3/search/movie?query=${encodeURIComponent(
        query
      )}&api_key=${apiKey}`;

      this.loading = true;
      this.error = null;

      try {
        const response = await axios.get(url, {
          headers: {
            accept: "application/json",
            Authorization: `Bearer ${bearerToken}`, // Include the Bearer token in headers
          },
        });

        // Update movies with fetched data
        this.movies = response.data.results;
      } catch (err) {
        this.error = "Error fetching data, please try again.";
        console.error(err.response ? err.response.data : err.message); // Log detailed error information
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>
