<template>
  <header class="bg-indigo-600">
    <nav class="mx-auto max-w-7xl px-6 lg:px-8" aria-label="Top">
      <div
        class="flex w-full items-center justify-between border-b border-indigo-500 py-6 lg:border-none"
      >
        <div class="flex items-center">
          <p class="text-xl font-bold text-white">Movie Search</p>
        </div>
        <div class="ml-10 space-x-4">
          <div class="mt-2 flex rounded-md shadow-sm">
            <div
              class="relative flex flex-grow items-stretch focus-within:z-10"
            >
              <div
                class="pointer-events-none absolute inset-y-0 left-0 flex items-center pl-3"
              >
                <UsersIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
              </div>
              <input
                type="email"
                name="email"
                id="email"
                class="block w-full rounded-none rounded-l-md border-0 py-1.5 pl-10 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm/6"
                placeholder="John Smith"
              />
            </div>
            <button
              type="button"
              class="relative -ml-px inline-flex items-center gap-x-1.5 rounded-r-md px-3 py-2 text-sm font-semibold text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50"
            >
              Sort
            </button>
          </div>
        </div>
      </div>
    </nav>
  </header>
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
