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
    searchTitle() {
      if (!this.titleFilter) {
        store.movies = [];
        store.series = [];
        return;
      };
      axios.get(`${api.baseUri}/search/movie?api_key=${api.key}&language=${api.language}&query=${this.titleFilter}`)
        .then(res => {
          store.movies = res.data.results;
        })
      axios.get(`${api.baseUri}/search/tv?api_key=${api.key}&language=${api.language}&query=${this.titleFilter}`)
        .then(res => {
          store.series = res.data.results;
        })
    }
  }
}

</script>

<template>
  <searchbar @term-change="setTitleFilter" @form-submit="searchTitle" />
  <h1>MOVIES</h1>
  <ul v-for="movie in store.movies">
    <li>{{ movie.title }}</li>
    <li>{{ movie.original_title }}</li>
    <li>{{ movie.original_language }}</li>
    <li>{{ movie.vote_average }}</li>
  </ul>
  <h1>SERIES</h1>
  <ul v-for="serie in store.series">
    <li>{{ serie.name }}</li>
    <li>{{ serie.original_name }}</li>
    <li>{{ serie.original_language }}</li>
    <li>{{ serie.vote_average }}</li>
  </ul>
</template>

<style></style>