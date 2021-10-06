<template>
    <section>
        <div class="container">
            <div class="row">
                <div v-if="tvsArray.length != 0" class="d-flex justify-content-between align-items-center">
                    <h2 v-if="moviesArray.length != 0">Movies:</h2>
                    <h2 v-else-if="search">There's no match on movies</h2>
                    <div>
                        <label for="selectGenreMovies">Select genre: </label>
                        <select name="selectGenreMovies" id="selectGenreMovies" v-model="moviesGenreSelected">
                            <option value="">All genres</option>
                            <option v-for="genre in moviesGenres" :key="genre.id" :value="genre.id">{{genre.name}}</option>
                        </select>
                    </div>
                </div>
                <Card class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="movie in moviesFiltered" :key="movie.id" :info="movie" :whatIs="film"/>
                <h2 v-show="moviesFiltered.length == 0 && moviesArray.length != 0">There's no match on this genre</h2>
            </div>
            <div class="row">
                <div v-if="tvsArray.length != 0" class="d-flex justify-content-between align-items-center">
                    <h2 v-if="tvsArray.length != 0">Tv series:</h2>
                    <h2 v-else-if="search">There's no match on tv series</h2>
                    <div>
                        <label for="selectGenreMovies">Select genre: </label>
                        <select name="selectGenreTvs" id="selectGenreTvs" v-model="tvsGenreSelected">
                            <option value="">All genres</option>
                            <option v-for="genre in tvsGenres" :key="genre.id" :value="genre.id">{{genre.name}}</option>
                        </select>
                    </div>
                </div>
                <Card class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="tvs in tvsFiltered" :key="tvs.id" :info="tvs" :whatIs="tvS"/>
                <h2 v-show="tvsFiltered.length == 0 && tvsArray.length != 0">There's no match on this genre</h2>
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
            moviesGenres: [],
            tvsGenres: [],
            tvS: 'tvs',
            film: 'film',
            search: false,
            moviesGenreSelected: '',
            tvsGenreSelected: ''
        }
    },
    watch: {
        stringSearch: function() {
                this.getMoviesTvs();
                this.search = true;
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
    computed: {
        moviesFiltered() {
            let arrayFiltered;
            if (this.moviesGenreSelected == "") {
                arrayFiltered = this.moviesArray;
            } else {
                arrayFiltered = this.moviesArray.filter( (elm) => {
                    if ( elm.genre_ids.includes(this.moviesGenreSelected) ) {
                        return true;
                    }
                    return false;
                });
            }
            return arrayFiltered;
        },
        tvsFiltered() {
            let arrayFiltered;
            if (this.tvsGenreSelected == "") {
                arrayFiltered = this.tvsArray;
            } else {
                arrayFiltered = this.tvsArray.filter( (elm) => {
                    if ( elm.genre_ids.includes(this.tvsGenreSelected) ) {
                        return true;
                    }
                    return false;
                });
            }
            return arrayFiltered;
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

        axios.get('https://api.themoviedb.org/3/genre/movie/list', {
                params: {
                api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
                language: 'it-IT'
                }
            })
            .then( (resp) => {
                console.log(resp);
                this.moviesGenres = resp.data.genres;
            });

        axios.get('https://api.themoviedb.org/3/genre/tv/list', {
                params: {
                api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
                language: 'it-IT'
                }
            })
            .then( (resp) => {
                this.tvsGenres = resp.data.genres;
            });
    }
}
</script>

<style lang="scss" scoped>
section {
    margin-top: 3.515625rem;
    background-color: rgb(20, 20, 20);

    select {
        padding: .3125rem .625rem;
        border: none;
        border: .0625rem solid #fff;
        background-color: inherit;
        color: #fff;
    }

    label {
        color: #fff;
        margin-right: .3125rem;
    }
}
</style>