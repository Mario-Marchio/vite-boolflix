<script>
import axios from 'axios';
import { api } from './components/data/index'
import { store } from './components/data/store'
import searchbar from './searchbar.vue';
export default {
  components: { searchbar },
  data() {
    return {
      store,
      titleFilter: '',
    }
  },
  methods: {
    setTitleFilter(term) {
      this.titleFilter = term;
    },
    searchMovie() {
      if (!this.titleFilter) {
        store.movies = [];
        return;
      };
      axios.get(`${api.baseUri}/search/movie?api_key=${api.key}&language=${api.language}&query=${this.titleFilter}`)
        .then(res => {
          store.movies = res.data.results;
        })
    }
  }
}

</script>

<template>
  <searchbar @term-change="setTitleFilter" @form-submit="searchMovie" />
  <h1>MOVIES</h1>
  <ul v-for="movie in store.movies">
    <li>{{ movie.title }}</li>
    <li>{{ movie.original_title }}</li>
    <li>{{ movie.original_language }}</li>
    <li>{{ movie.vote_average }}</li>
  </ul>
</template>

<style></style>