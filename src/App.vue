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
  <div class="bg-secondary">
    <h1>MOVIES :</h1>
    <div class="row ">
      <ul v-for="movie in store.movies" :key="movie.id" class="card col-lg-3 m-2 bg-black">
        <div class="card-image" :style="{ backgroundImage: 'url(' + getPosterUrlM(movie) + ')' }">
          <div class="bgc-black-o">
            <div class="d-flex flex-column">
              <li class="mt-5"> <b>TITOLO : </b> {{ movie.title }}</li>
              <li class="mt-3"> <b>TITOLO ORIGINALE : </b>{{ movie.original_title }}</li>
              <div class="d-flex flex-row mt-3">
                <li><b>VOTO : </b></li>
                <li v-html="getRatingStars(movie)" class="rating-stars"></li>
              </div>
              <li>
                <img class="flag mt-3" v-if="hasFlagM(movie)" :src="flagScrM(movie)" :alt="movie.original_language">
                <span v-else>{{ movie.original_language }}</span>
              </li>
            </div>
          </div>
        </div>
      </ul>
      <h1>SERIES :</h1>
      <ul v-for="serie in store.series" :key="serie.id" class="card col-lg-3 m-2 bg-black">
        <div class="card-image" :style="{ backgroundImage: 'url(' + getPosterUrlS(serie) + ')' }">
          <div class="bgc-black-o ">
            <div class="d-flex flex-column">
              <li class="mt-5"><b>TITOLO : </b>{{ serie.name }}</li>
              <li class="mt-3"><b>TITOLO ORIGINALE : </b>{{ serie.original_name }}</li>
              <div class="d-flex flex-row mt-3">
                <li><b>VOTO : </b></li>
                <li v-html="getRatingStars(serie)" class="rating-stars"></li>
              </div>
              <li>
                <img class="flag mt-3" v-if="hasFlagS(serie)" :src="flagScrS(serie)" :alt="serie.original_language">
                <span v-else>{{ serie.original_language }}</span>
              </li>
            </div>
          </div>
        </div>
      </ul>
    </div>
  </div>
</template>

<style lang="scss">
ul {
  list-style-type: none;
}

li {
  color: white;
}

.rating-stars {
  color: yellow;
}

.card-image {
  height: 513px;
  width: 342px;
}

.p-0 {
  padding: 0px
}

.flag {
  display: block;
  height: 50px;

}

.bgc-black-o {
  background-color: black;
  opacity: 0.9;
  height: 513px;
  display: none;
}

.card:hover .bgc-black-o {
  display: block;
}

.card {
  width: 23%;
}
</style>