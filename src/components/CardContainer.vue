<template>
    <section>
        <div class="container-xxl ">
            <!-- Print searched movies -->
            <div id="moviesDivSearched" class="row">
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
            </div>
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5 row-cols-xxl-8">
                <Card class="col" v-for="movie in moviesFiltered" :key="movie.id" :info="movie" :whatIs="film"/>
                <h2 v-show="moviesFiltered.length == 0 && moviesArray.length != 0">There's no match on this genre</h2>
            </div>
            <!-- Print searched Tv Series -->
            <div id="tvsDivSearched" class="row">
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
            </div>
            <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5 row-cols-xxl-8">
                <Card class="col" v-for="tvs in tvsFiltered" :key="tvs.id" :info="tvs" :whatIs="tvS"/>
                <h2 v-show="tvsFiltered.length == 0 && tvsArray.length != 0">There's no match on this genre</h2>
            </div>
            <!-- Print popular movies -->
            <div id="moviesDiv" class="row">
                <h2 v-if="popularMovies.length != 0">Popular movies:</h2>
            </div>
            <div  class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5 row-cols-xxl-8">
                <Card class="col" v-for="movie in popularMovies" :key="movie.id" :info="movie" :whatIs="film"/>
            </div>
            <!-- Print polpular Tv Series -->
            <div id="tvsDiv" class="row">
                <h2 v-if="popularTvs.length != 0">Popular Tv Series:</h2>
            </div>
            <div  class="row row-cols-1 row-cols-sm-2 row-cols-md-3 row-cols-lg-4 row-cols-xl-5 row-cols-xxl-8">
                <Card class="col" v-for="tvs in popularTvs" :key="tvs.id" :info="tvs" :whatIs="tvS"/>
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
        stringSearch: String,
        resetString: String
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
            tvsGenreSelected: '',
            whatIs: '',
            api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
            video: ''
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
            if ( this.stringSearch == '' ) {
                this.moviesArray = [];
                this.tvsArray = [];
            }
            // Axios request for searched movies
            axios.get('https://api.themoviedb.org/3/search/movie', {
                params: {
                    api_key: this.api_key,
                    query: this.stringSearch,
                    language: 'it-IT'
                }
            })
            .then( (resp) => {
                this.moviesArray = resp.data.results;
            });

            // Axios request for searched Tv Series 
            axios.get('https://api.themoviedb.org/3/search/tv', {
                params: {
                    api_key: this.api_key,
                    query: this.stringSearch,
                    language: 'it-IT'
                }
            })
            .then( (resp) => {
                this.tvsArray = resp.data.results;
            });
        },
        playTrailer(video) {
            this.video = video;
        }
    },
    computed: {
        // Function for filter the searched movies 
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
        // Function for filter the searched Tv Series 
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
        // Axios request for Popular Movies 
        axios.get('https://api.themoviedb.org/3/movie/popular?page=1', {
                params: {
                api_key: this.api_key,
                language: 'it-IT'
                }
            })
            .then( (resp) => {
                this.popularMovies = resp.data.results;
            });

        // Axios request for Popular Tv Series 
        axios.get('https://api.themoviedb.org/3/tv/popular?page=1', {
                params: {
                    api_key: this.api_key,
                    language: 'it-IT'
                }
            })
            .then( (resp) => {
                this.popularTvs = resp.data.results;
            });

        // Axios request for movies genres 
        axios.get('https://api.themoviedb.org/3/genre/movie/list', {
                params: {
                    api_key: this.api_key,
                    language: 'it-IT'
                }
            })
            .then( (resp) => {
                this.moviesGenres = resp.data.genres;
            });

        // Axios request for Tv series genres 
        axios.get('https://api.themoviedb.org/3/genre/tv/list', {
                params: {
                    api_key: this.api_key,
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
    padding-bottom: 3.125rem;
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