<template>
  <div>
    <h2>Movie List</h2>
    <div v-if="loading">Loading movies...</div>
    <div v-if="error">{{ error }}</div>
    <ul>
      <li v-for="movie in movies" :key="movie.id">
        <h3>{{ movie.title }}</h3>
        <p>Release Date: {{ movie.release_date }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import BarcodeUser from "./Barcode.vue";

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
      const apiKey = "YOUR_API_KEY"; // Replace with your TMDb API key
      const bearerToken = "YOUR_BEARER_TOKEN"; // This is your Bearer token
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
