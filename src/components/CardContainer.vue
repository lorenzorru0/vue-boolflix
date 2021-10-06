<template>
    <section>
        <div class="container">
            <div class="row">
                <h2 v-if="moviesArray.length != 0">Movies:</h2>
                <!-- <h2 v-else>There's no match on movies</h2> -->
                <Card class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="movie in moviesArray" :key="movie.id" :info="movie" :whatIs="film"/>
            </div>
            <div class="row">
                <h2 v-if="tvsArray.length != 0">Tv series:</h2>
                <!-- <h2 v-else>There's no match on tv series</h2> -->
                <Card class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="tvs in tvsArray" :key="tvs.id" :info="tvs" :whatIs="tvS"/>
            </div>
            <div class="row">
                <h2 v-if="popularMovies.length != 0">Popular movies:</h2>
                <Card class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="movie in popularMovies" :key="movie.id" :info="movie" :whatIs="film"/>
            </div>
            <div class="row">
                <h2 v-if="popularTvs.length != 0">Popular Tv Series:</h2>
                <Card class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="tvs in popularTvs" :key="tvs.id" :info="tvs" :whatIs="tvS"/>
            </div>
        </div>
    </section>
</template>

<script>
import axios from 'axios';
import Card from './Card.vue'; 

export default {
    name: 'CardContainer',
    components: {
        Card
    },
    props: {
        stringSearch: String
    },
    data() {
        return {
            popularMovies: [],
            popularTvs: [],
            moviesArray: [],
            tvsArray: [],
            tvS: 'tvs',
            film: 'film'
        }
    },
    watch: {
        stringSearch: function() {
                this.getMoviesTvs();
            }
    },
    methods: {
        getMoviesTvs() {
            axios.get('https://api.themoviedb.org/3/search/movie', {
                params: {
                api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
                query: this.stringSearch,
                language: 'it-IT'
                }
            })
            .then( (resp) => {
                this.moviesArray = resp.data.results;
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

<style lang="scss" scoped>
section {
    margin-top: 3.515625rem;
    background-color: rgb(20, 20, 20);
}
</style>