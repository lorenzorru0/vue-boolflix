<template>
    <section>
        <div class="container">
            <div class="row">
                <h2 v-if="filmsArray.length != 0">Movies:</h2>
                <CardFilm class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="film in filmsArray" :key="film.id" :film="film"/>
            </div>
            <div class="row">
                <h2 v-if="tvsArray.length != 0">Tv series:</h2>
                <CardTvs class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="tvs in tvsArray" :key="tvs.id" :tvs="tvs"/>
            </div>
        </div>
    </section>
</template>

<script>
import CardFilm from './CardFilm.vue'; 
import CardTvs from './CardTvs.vue'; 

export default {
    name: 'CardContainer',
    components: {
        CardFilm,
        CardTvs
    },
    props: {
        filmsArray: Array,
        tvsArray: Array
    }
}
</script>

<style lang="scss" scoped>
section {
    margin-top: 3.515625rem;
    background-color: rgb(20, 20, 20);


    .container {

        h2 {
            margin: {
                top: 3.125rem;
                bottom: 1.5625rem;
            }
            color: #fff;
        }    
    }
}
</style>