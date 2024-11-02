<template>
  <div>
    <h2>Movie List</h2>
    <input
      v-model="searchQuery"
      placeholder="Search for a movie..."
      @keyup.enter="fetchMovies"
    />
    <button @click="fetchMovies">Search</button>

    <div v-if="loading">Loading...</div>
    <div v-if="error">{{ error }}</div>

    <ul>
      <li v-for="movie in movies" :key="movie.id">
        <h3>{{ movie.title }}</h3>
        <p>Release Date: {{ movie.release_date }}</p>
        <p>{{ movie.overview }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "MovieList",
  data() {
    return {
      movies: [],
      searchQuery: "",
      loading: false,
      error: null,
    };
  },
  methods: {
    async fetchMovies() {
      if (!this.searchQuery) return;
      const apiKey = "7bd3f902e99719c5aa57356eb3acb07e";
      const url = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${this.searchQuery}`;

      this.loading = true;
      this.error = null;
      try {
        const response = await axios.get(url);
        this.movies = response.data.results;
      } catch (err) {
        this.error = "Error fetching data, please try again.";
        console.error(err);
      } finally {
        this.loading = false;
      }
    },
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
