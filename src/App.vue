<template>
  <div id="app" >
    <Header @stringSearch="getStringSearch" />
    <CardContainer :filmsArray="filmsArray" :tvsArray="tvsArray" />
    <div class="container">
      <div class="row">
        <h2 v-if="popularMovies.length != 0">Popular movies:</h2>
        <CardFilm class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="movie in popularMovies" :key="movie.id" :film="movie"/>
      </div>
      <div class="row">
        <h2 v-if="popularTvs.length != 0">Popular Tv Series:</h2>
        <CardTvs class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="tvs in popularTvs" :key="tvs.id" :tvs="tvs"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Header from './components/Header.vue';
import CardContainer from './components/CardContainer.vue';
import CardFilm from './components/CardFilm.vue';
import CardTvs from './components/CardTvs.vue';

export default {
  name: 'App',
  components: {
    Header,
    CardContainer,
    CardFilm,
    CardTvs
  },
  data() {
    return {
      stringSearch: '',
      filmsArray: [],
      tvsArray: [],
      popularMovies: [],
      popularTvs: []
    }
  },
  methods: {
    getStringSearch(stringSearch) {
      this.stringSearch = stringSearch;
      axios.get('https://api.themoviedb.org/3/search/movie', {
        params: {
          api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
          query: this.stringSearch,
          language: 'it-IT'
        }
      })
      .then( (resp) => {
        this.filmsArray = resp.data.results;
      });

      axios.get('https://api.themoviedb.org/3/search/tv', {
        params: {
          api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
          query: this.stringSearch,
          language: 'it-IT'
        }
      })
      .then( (resp) => {
        this.tvsArray = resp.data.results;
      });
    }
  },
  created() {
    axios.get('https://api.themoviedb.org/3/movie/popular?page=1', {
        params: {
          api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
          language: 'it-IT'
        }
      })
      .then( (resp) => {
        for (let i = 0; i < 8; i++) {
          this.popularMovies.push(resp.data.results[i]);
        }
      });
    axios.get('https://api.themoviedb.org/3/tv/popular?page=1', {
        params: {
          api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
          language: 'it-IT'
        }
      })
      .then( (resp) => {
        for (let i = 0; i < 8; i++) {
          this.popularTvs.push(resp.data.results[i]);
        }
      });
  }
}
</script>

<style lang="scss">
@import './assets/style/common.scss';

#app {
  min-height: calc(100vh - 4.765625rem);
  background-color: rgb(20, 20, 20);
  margin-top: 4.765625rem;

  h2 {
    margin: {
        top: 1.5625rem;
        bottom: 1.5625rem;
    }
    color: #fff;
  }
}
</style>
