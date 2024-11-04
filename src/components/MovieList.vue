<template>
  <div>
    <h2>Movie List</h2>
    <div v-if="loading">Loading movies...</div>
    <div v-if="error">{{ error }}</div>
    <ul>
      <li v-for="movie in movies" :key="movie.id">
        <h3>{{ movie.title }}</h3>
        <p>Release Date: {{ movie.release_date }}</p>
        <BarcodeUser :value="movie.id.toString()" />
        <!-- Display barcode for each movie -->
      </li>
    </ul>
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
  data() {
    return {
      movies: [],
      loading: false,
      error: null,
    };
  },
  methods: {
    async fetchMovies() {
      const apiKey = process.env.VUE_APP_API_KEY; // Replacing with VUE_APP_ for Vue CLI variable
      const bearerToken = process.env.VUE_APP_API_BEARER_TOKEN; // Ensure it starts with VUE_APP_ in .env

      const url = `https://api.themoviedb.org/3/movie/popular?api_key=${apiKey}`;

      this.loading = true;
      this.error = null;

      try {
        const response = await axios.get(url, {
          headers: {
            Authorization: `Bearer ${bearerToken}`, // Set the Bearer token here
          },
        });
        this.movies = response.data.results;
      } catch (err) {
        this.error = "Error fetching data, please try again.";
        console.error(err);
      } finally {
        this.loading = false;
      }
    },
  },
  created() {
    this.fetchMovies();
  },
};
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin-bottom: 20px;
}
</style>
