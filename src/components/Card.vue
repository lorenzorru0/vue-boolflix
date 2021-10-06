<template>
    <div @mouseover="hovered = true" @mouseleave="hovered = false">
        <img :src="'https://image.tmdb.org/t/p/w342' + info.poster_path" alt="Film poster" :class="hovered == true ? 'opacity0' : 'opacity1'">
        <ul :class="hovered == true ? 'opacity1ul' : 'opacity0ul'">
            <li v-if="whatIs == 'film'"> <strong>Title:</strong> {{info.title}}</li>
            <li v-else> <strong>Name:</strong> {{info.name}}</li>
            <li v-if="whatIs == 'film'"> <strong>Original title:</strong> {{info.original_title}}</li>
            <li v-else> <strong>Original name:</strong> {{info.original_name}}</li>
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
            <li> <strong>Overview: </strong> {{info.overview}}</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'CardFilm',
    props: {
        info: Object,
        whatIs: String
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