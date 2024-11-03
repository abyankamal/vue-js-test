<template>
  <div class="mb-4">
    <input
      v-model="searchQuery"
      type="text"
      placeholder="Search for a movie..."
      @keyup.enter="emitSearch"
      class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm/6"
    />
    <button @click="emitSearch" class="mt-2 bg-blue-500 text-white p-2 rounded">
      Search
    </button>
  </div>
</template>

<script>
export default {
  name: "MovieSearch",
  data() {
    return {
      searchQuery: "", // Holds user input from the search bar
    };
  },
  methods: {
    emitSearch() {
      // Emit an event with the search query to the parent component
      if (this.searchQuery.trim()) {
        this.$emit("search", this.searchQuery); // Emit the query if not empty
        this.searchQuery = ""; // Clear the input after emitting
      }
    },
  },
};
</script>

<style scoped>
/* Add component-specific styles here if needed */
</style>
