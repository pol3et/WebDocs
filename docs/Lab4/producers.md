# Producers.vue

## Producers Companies Page Component

```html
<template>
  <div>
    <h1>Producers Companies Page</h1>
    
    <!-- Search input -->
    <label>Search by Name:</label>
    <input v-model="searchTerm" @input="filterProducers" />

    <!-- List of producers -->
    <ul>
      <li v-for="producer in filteredProducers" :key="producer.id">
        {{ producer.name }}
      </li>
    </ul>
  </div>
</template>
```

- **Description**: Vue template for the Producers Companies page component. Displays a title, a search input, and a list of producers.

## Producers Companies Page Component Script

```html
<script>
import axios from 'axios';

export default {
  name: 'Producers',
  data() {
    return {
      producers: [],
      searchTerm: '',
    };
  },
  computed: {
    filteredProducers() {
      const search = this.searchTerm.toLowerCase();
      return this.producers.filter(
        producer => producer.name.toLowerCase().includes(search)
      );
    },
  },
  methods: {
    async fetchProducers() {
      try {
        const response = await axios.get('http://127.0.0.1:8000/main/companies/', {
          headers: {
            Authorization: `Token ${localStorage.getItem('access_token')}`,
          },
        });
        this.producers = response.data;
      } catch (error) {
        console.error('Error fetching producer data:', error);
      }
    },
    filterProducers() {
      // Triggered when search term changes
    },
  },
  created() {
    this.fetchProducers();
  },
};
</script>
```

- **Description**: Vue script for the Producers Companies page component. Manages data, computed properties, methods for fetching and filtering producers.

## [Products.vue](products.md) is up next!