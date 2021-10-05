<template>
    <div @mouseover="hovered = true" @mouseleave="hovered = false">
        <img :src="'https://image.tmdb.org/t/p/w342' + film.poster_path" alt="Film poster" :class="hovered == true ? 'opacity0' : 'opacity1'">
        <ul :class="hovered == true ? 'opacity1ul' : 'opacity0ul'">
            <li> <strong>Title:</strong> {{film.title}}</li>
            <li> <strong>Original title:</strong> {{film.original_title}}</li>
            <li v-if="film.original_language == 'en'"> <strong>Original language: </strong> <img src="https://www.countryflags.io/us/flat/64.png" alt="Language img"></li>
            <li v-else> <strong>Original language: </strong> <img :src="'https://www.countryflags.io/' + film.original_language + '/flat/64.png'" alt="Language img"></li>
            <li>
                <strong>Vote: </strong> 
                <template v-if="film.vote_average != 0">
                    <i class="fas fa-star" v-for="(icon, index) in parseInt(film.vote_average / 2)" :key="index"></i>
                    <i class="far fa-star" v-for="(icon, index) in parseInt(6 - film.vote_average / 2)" :key="'ligth' + index"></i>
                </template>
                <template v-else>
                    <i class="far fa-star" v-for="(icon, index) in 5" :key="'ligth' + index"></i>
                </template>
            </li>
            <li> <strong>Overview: </strong> {{film.overview}}</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'CardFilm',
    props: {
        film: Object
    },
    data() {
        return {
            hovered: false
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/style/variable.scss';

@include card;
</style>