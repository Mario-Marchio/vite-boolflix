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
    },
    hasFlagM(movie) {
      const avaibleFlags = ['it', 'en'];
      return avaibleFlags.includes(movie.original_language)
    },

    flagScrM(movie) {
      const url = new URL(`./assets/${movie.original_language}.png`, import.meta.url);
      return url.href
    },
    getPosterUrlM(movie) {
      return `https://image.tmdb.org/t/p/w342/${movie.poster_path}`;
    },
    getRatingStars(movie) {
      const rating = Math.ceil(movie.vote_average / 2);
      const stars = [];
      for (let i = 1; i <= 5; i++) {
        if (i <= rating) {
          stars.push('<i class="fa-solid fa-star"></i>');
        } else {
          stars.push('<i class="fa-regular fa-star"></i>');
        }
      }
      return stars.join('');
    },
    hasFlagS(serie) {
      const avaibleFlags = ['it', 'en'];
      return avaibleFlags.includes(serie.original_language)
    },

    flagScrS(serie) {
      const url = new URL(`./assets/${serie.original_language}.png`, import.meta.url);
      return url.href
    },
    getPosterUrlS(serie) {
      return `https://image.tmdb.org/t/p/w342/${serie.poster_path}`;
    },
    getRatingStars(serie) {
      const rating = Math.ceil(serie.vote_average / 2);
      const stars = [];
      for (let i = 1; i <= 5; i++) {
        if (i <= rating) {
          stars.push('<i class="fa-solid fa-star"></i>');
        } else {
          stars.push('<i class="fa-regular fa-star"></i>');
        }
      }
      return stars.join('');
    }
  }
}

</script>

<template>
  <appHeader />
  <searchbar @term-change="setTitleFilter" @form-submit="searchTitle" />
  <h1>MOVIES</h1>
  <ul v-for="movie in store.movies">
    <li>{{ movie.title }}</li>
    <li>{{ movie.original_title }}</li>
    <li>
      <img v-if="hasFlagM(movie)" :src="flagScrM(movie)" :alt="movie.original_language">
      <span v-else>{{ movie.original_language }}</span>
    </li>
    <li v-html="getRatingStars(movie)"></li>
    <li><img :src="getPosterUrlM(movie)" :alt="movie.title" /></li>
  </ul>
  <h1>SERIES</h1>
  <ul v-for="serie in store.series">
    <li>{{ serie.name }}</li>
    <li>{{ serie.original_name }}</li>
    <li>
      <img v-if="hasFlagS(serie)" :src="flagScrS(serie)" :alt="serie.original_language">
      <span v-else>{{ serie.original_language }}</span>
    </li>
    <li v-html="getRatingStars(serie)"></li>
    <li><img :src="getPosterUrlS(serie)" :alt="serie.title" /></li>
  </ul>
</template>

<style></style>