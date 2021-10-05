<template>
    <div>
        <ul>
            <li>{{film.title}}</li>
            <li>{{film.original_title}}</li>
            <li v-if="film.original_language == 'en'"> <img src="https://www.countryflags.io/us/flat/64.png" alt="Language img"></li>
            <li v-else><img :src="'https://www.countryflags.io/' + film.original_language + '/flat/64.png'" alt="Language img"></li>
            <li>{{film.vote_average}}</li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'Card',
    props: {
        film: Object
    }
}
</script>

<style lang="scss" scoped>
div {
    ul {
        img {
            width: 2rem;
        }
    }
}
</style>