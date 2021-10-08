<template>
  <div id="app">
    <!-- Header  -->
    <Header @stringSearch="getStringSearch" @resetSearch="resetSearch" />
    <!-- Trailer popUp  -->
    <div v-if="video.view" class="popUp" v-scroll-lock="true">
      <div class="popUpBackground"></div>
      <div class="videoContainer">
        <div class="closeIcon" @click="closeVideo()">
          <i class="fas fa-times"></i>
        </div>
        <iframe :src="`https://www.youtube.com/embed/${videoId}`" frameborder="0" allowfullscreen></iframe>
      </div>
    </div>
    <!-- Card container  -->
    <CardContainer :stringSearch="stringSearch" :resetString="resetString" />
  </div>
</template>

<script>
import Vue from 'vue'
import Header from './components/Header.vue';
import CardContainer from './components/CardContainer.vue';
import video from './observable/video';
import VScrollLock from 'v-scroll-lock';

Vue.use(VScrollLock);


export default {
  name: 'App',
  components: {
    Header,
    CardContainer
  },
  data() {
    return {
      stringSearch: '',
      resetString: '',
      video
    }
  },
  methods: {
    getStringSearch(stringSearch) {
      this.stringSearch = stringSearch;
    },
    resetSearch(reset) {
      this.resetString = reset;
    },
    closeVideo() {
      video.view = false;
    }
  },
  computed: {
    videoId() {
      return video.video;
    }
  }
}
</script>

<style lang="scss">
@import '~@fortawesome/fontawesome-free/css/all.css';
@import './assets/style/common.scss';

#app {
  position: relative;
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
  .popUp {
    position: fixed;
    width: 100%;
    height: 100vh;
    top: 0;
    left: 0;
    z-index: 2;
    display: flex;
    justify-content: center;
    align-items: center;

    .popUpBackground {
      position: fixed;
      width: 100%;
      height: 100vh;
      top: 0;
      left: 0;
      background-color: rgba(0,0,0,.4);
    }

    .videoContainer {
      position: relative;
      z-index: 3;
      width: 46.875rem;
      height: 21.875rem;

      .closeIcon {
        position: absolute;
        width: 3.125rem;
        height: 3.125rem;
        display: flex;
        justify-content: center;
        align-items: center;
        border: .0625rem solid #fff;
        color: #fff;
        font-size: 2rem;
        right: -3.125rem;
        cursor: pointer;
      }

      iframe {
        width: 100%;
        height: 100%;
      }
    }
  }
}
</style>
