<template>
  <div id="app">
    <Header @stringSearch="getStringSearch" />
    <CardContainer :filmsArray="filmsArray" />
  </div>
</template>

<script>
import axios from 'axios';
import Header from './components/Header.vue';
import CardContainer from './components/CardContainer.vue';

export default {
  name: 'App',
  components: {
    Header,
    CardContainer
  },
  data() {
    return {
      stringSearch: '',
      filmsArray: []
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
    }
  }
}
</script>

<style lang="scss">
@import './assets/style/common.scss';
</style>
