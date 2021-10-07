<template>
    <div @mouseleave="addInfoBool = false">
        <img v-if="info.poster_path != null" :src="'https://image.tmdb.org/t/p/w342' + info.poster_path" alt="Film poster">
        <ul :class="info.poster_path == null ? 'opacity1ul' : null ">
            <li> <strong>Title:</strong> {{info.title || info.name}}</li>
            <li> <strong>Original title:</strong> {{info.original_title || info.original_name}}</li>
            <li> <strong>Original language: </strong> <img :src="'https://www.unknown.nu/flags/images/' + info.original_language + '-100'" alt="Language img"></li>
            <li>
                <strong>Vote: </strong> 
                <template v-if="info.vote_average != 0">
                    <i class="fas fa-star" v-for="(icon, index) in parseInt(info.vote_average / 2)" :key="index"></i>
                    <i class="far fa-star" v-for="(icon, index) in parseInt(6 - info.vote_average / 2)" :key="'ligth' + index"></i>
                </template>
                <template v-else>
                    <i class="far fa-star" v-for="(icon, index) in 5" :key="'ligth' + index"></i>
                </template>
            </li>
            <li v-if="info.overview != '' || actors.length != 0 || genres.length != 0"><strong>Click the i for more info</strong> <i class="fas fa-info-circle" @click="addInfo()"></i> </li>
            <template v-if="addInfoBool">
                <li v-if="info.overview != ''"> <strong>Overview: </strong> {{info.overview}}</li>
                <template v-if="actors.length != 0">
                    <li> <strong>Popular actors: </strong></li>
                    <li v-for="actor in actors" :key="actor.id"> <strong>-</strong> {{actor}}</li>
                </template>
                <template v-if="genres.length != 0">
                    <li> <strong>Genres: </strong></li>
                    <li v-for="genre in genres" :key="genre.id"> <strong>-</strong> {{genre}}</li>
                </template>
            </template>
        </ul>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'CardFilm',
    props: {
        info: Object,
        whatIs: String
    },
    data() {
        return {
            actors: [],
            genres: [],
            video: '',
            addInfoBool: false
        }
    },
    methods: {
        addInfo() {
            this.addInfoBool = !this.addInfoBool;
        }
    },
    created() {
        axios.get(`https://api.themoviedb.org/3/movie/${this.info.id}/credits` , {
                params: {
                    api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
                    language: 'it-IT'
                }
            })
            .then( (resp) => {
                for (let i = 0; i < 5; i++) {
                    this.actors.push(resp.data.cast[i].original_name);
                }
            })
            .catch( () => {
                console.log("No actors have been added yet!" );
            });
        
        axios.get(`https://api.themoviedb.org/3/movie/${this.info.id}` , {
                params: {
                    api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
                    language: 'it-IT'
                }
            })
            .then( (resp) => {
                for (let i = 0; i < resp.data.genres.length; i++) {
                    this.genres.push(resp.data.genres[i].name);
                }
            })
            .catch( () => {
                console.log('No genres have been added yet!!');
            });

        if ( this.whatIs == 'film') {
            axios.get(`https://api.themoviedb.org/3/movie/${this.info.id}/videos` , {
                params: {
                    api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
                    language: 'it-IT'
                }
            })
            .then( (resp) => {
                console.log(resp);
                this.video = resp.data.results[0].key;
            })
            .catch( () => {
                console.log("No video have been added yet!" );
            });
        } else {
            axios.get(`https://api.themoviedb.org/3/tv/${this.info.id}/videos` , {
                params: {
                    api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
                    language: 'it-IT'
                }
            })
            .then( (resp) => {
                console.log(resp);
                this.video = resp.data.results[0].key;
            })
            .catch( () => {
                console.log("No video have been added yet!" );
            });
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/style/variable.scss';

div {
    position: relative;
    margin-bottom: 1.25rem;
    overflow-y: auto;

    &:hover > img {
        opacity: 0;
        transition-delay:0.3s;
    }
    &:hover > ul {
        visibility: visible;
        opacity: 1;
        transition-delay:0.3s;
    }

    img {
        width: 100%;
        opacity: 1;
        transition: opacity 0.3s;
    }

    .opacity1ul {
        visibility: visible;
        opacity: 1;
    }
        
    ul {
        list-style: none;
        padding-left: 0;
        position: absolute;
        top: 0;
        color: #fff;
        padding: 1.25rem .9375rem;
        visibility: hidden;
        opacity: 0;
        transition: visibility 0s, opacity 0.5s linear;

        img {
            width: 2rem;
        }

        i {
            color: gold;
        }

        .fa-info-circle {
            margin: {
                top: .3125rem;
                bottom: .3125rem;
                left: .3125rem;
            };
            color: #fff;
            font-size: 2rem;
            vertical-align: middle;
            cursor: pointer;
        }
    }
}
</style>