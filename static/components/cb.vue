<template>
  <div>
    <form @submit.prevent="search">
      <input type="text" v-model="searchQuery" placeholder="Search...">
      <button type="submit">Search</button>
    </form>

    <div v-if="isLoading">Loading...</div>
    <div v-else-if="error">{{ error }}</div>
    <div v-else-if="results.length === 0">No results found.</div>
    <div v-else>
      <ul>
        <li v-for="result in results" :key="result.id">
          {{ result.title }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: '',
      isLoading: false,
      error: null,
      results: [],
    };
  },
  methods: {
    search() {
      this.isLoading = true;
      this.results = [];
      this.error = null;

      fetch(`https://example.com/api/search?q=${this.searchQuery}`)
        .then(response => response.json())
        .then(data => {
          this.results = data.results;
          this.isLoading = false;
        })
        .catch(error => {
          this.error = 'An error occurred while searching. Please try again later.';
          this.isLoading = false;
        });
    },
  },
};
</script>
