<template>
    <div @mouseleave="addInfoBool = false">
        <!-- Print the poster path -->
        <img v-if="info.poster_path != null" :src="'https://image.tmdb.org/t/p/w342' + info.poster_path" alt="Film poster">
        <!-- Print the normal info -->
        <ul :class="info.poster_path == null ? 'opacity1ul' : null ">
            <li> <strong>Title:</strong> {{info.title || info.name}}</li>
            <li> <strong>Original title:</strong> {{info.original_title || info.original_name}}</li>
            <li> <strong>Original language: </strong> <img :src="'https://www.unknown.nu/flags/images/' + info.original_language + '-100'" alt="Language img"></li>
            <li>
                <strong>Vote: </strong> 
                <i class="fas fa-star" v-for="(icon, index) in Math.ceil(info.vote_average / 2)" :key="index"></i>
                <i class="far fa-star" v-for="(icon, index) in Math.ceil(5 - Math.ceil(info.vote_average / 2))" :key="'ligth' + index"></i>
            </li>
            <li v-if="videoId != ''">
                <div @click="playVideo()" >TRAILER</div>
            </li>
            <!-- Print more specific info -->
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
import video from '../observable/video';

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
            videoId: '',
            addInfoBool: false,
            video
        }
    },
    methods: {
        addInfo() {
            this.addInfoBool = !this.addInfoBool;
        },
        playVideo() {
            video.video = this.videoId;
            video.view = true;
        }
    },
    created() {
        // Axios request for the actors
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
        
        // Axios request for the genres 
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

        // Axios request for the videos 
        if ( this.whatIs == 'film') {
            axios.get(`https://api.themoviedb.org/3/movie/${this.info.id}/videos` , {
                params: {
                    api_key: 'ed7970cf990eb8d2f2cdf5a51640ead4',
                    language: 'it-IT'
                }
            })
            .then( (resp) => {
                this.videoId = resp.data.results[0].key;
            })
        }
    }
}
</script>

<style lang="scss" scoped>
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

        div {
            display: inline-block;
            margin: .3125rem auto;
            text-decoration: none;
            color: #fff;
            padding: .3125rem;
            border: .0625rem solid #fff;
            cursor: pointer;
        }
    }
}
</style>