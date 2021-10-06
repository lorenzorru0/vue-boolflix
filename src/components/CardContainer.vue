<template>
    <section>
        <div class="container">
            <div class="row">
                <h2 v-if="filmsArray.length != 0">Movies:</h2>
                <!-- <h2 v-else>There's no match on movies</h2> -->
                <CardFilm class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="film in filmsArray" :key="film.id" :film="film"/>
            </div>
            <div class="row">
                <h2 v-if="tvsArray.length != 0">Tv series:</h2>
                <!-- <h2 v-else>There's no match on tv series</h2> -->
                <CardTvs class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="tvs in tvsArray" :key="tvs.id" :tvs="tvs"/>
            </div>
            <div class="row">
                <h2 v-if="popularMovies.length != 0">Popular movies:</h2>
                <CardFilm class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="movie in popularMovies" :key="movie.id" :film="movie"/>
            </div>
            <div class="row">
                <h2 v-if="popularTvs.length != 0">Popular Tv Series:</h2>
                <CardTvs class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="tvs in popularTvs" :key="tvs.id" :tvs="tvs"/>
            </div>
        </div>
    </section>
</template>

<script>
import axios from 'axios';
import CardFilm from './CardFilm.vue'; 
import CardTvs from './CardTvs.vue'; 

export default {
    name: 'CardContainer',
    components: {
        CardFilm,
        CardTvs
    },
    props: {
        stringSearch: String
    },
    data() {
        return {
            popularMovies: [],
            popularTvs: [],
            filmsArray: [],
            tvsArray: []
        }
    },
    watch: {
        stringSearch: {
            handler: function() {
                this.getMoviesTvs();
            }
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

<style lang="scss" scoped>
section {
    margin-top: 3.515625rem;
    background-color: rgb(20, 20, 20);
}
</style>