<template>
  <div id="app" >
    <Header @stringSearch="getStringSearch" />
    <CardContainer :stringSearch="stringSearch" />
  </div>
</template>

<script>
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
      stringSearch: ''
    }
  },
  methods: {
    getStringSearch(stringSearch) {
      this.stringSearch = stringSearch;
    }
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
