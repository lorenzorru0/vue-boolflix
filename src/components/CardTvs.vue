<template>
    <div @mouseover="hovered = true" @mouseleave="hovered = false">
        <img :src="'https://image.tmdb.org/t/p/w342' + tvs.poster_path" alt="Tvs Poster" :class="hovered == true ? 'opacity0' : 'opacity1'">
        <ul :class="hovered == true ? 'opacity1ul' : 'opacity0ul'">
            <li> <strong>Name: </strong> {{tvs.name}}</li>
            <li> <strong>Original name: </strong> {{tvs.original_name}}</li>
            <li v-if="tvs.original_language == 'en'">  <strong>Original language: </strong> <img src="https://www.countryflags.io/us/flat/64.png" alt="Language img"></li>
            <li v-else> <strong>Original language: </strong> <img :src="'https://www.countryflags.io/' + tvs.original_language + '/flat/64.png'" alt="Language img"></li>
            <li>
                <strong>Vote: </strong> 
                <template v-if="tvs.vote_average != 0">
                    <i class="fas fa-star" v-for="(icon, index) in parseInt(tvs.vote_average / 2)" :key="index"></i>
                    <i class="far fa-star" v-for="(icon, index) in parseInt(6 - tvs.vote_average / 2)" :key="'ligth' + index"></i>
                </template>
                <template v-else>
                    <i class="far fa-star" v-for="(icon, index) in 5" :key="'ligth' + index"></i>
                </template>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'CardTvs',
    props: {
        tvs: Object
    },
    data() {
        return {
            hovered: false
        }
    },
    methods: {
        mouseover() {
            setTimeout(() => {this.hovered = true}, 2000);
        },
        mouseleave() {
            setTimeout(() => {this.hovered = false}, 500);
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../assets/style/variable.scss';

@include card;
</style>
