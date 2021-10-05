<template>
    <section>
        <div class="container">
            <div class="row">
                <Card class="col-4" v-for="film in filmsArray" :key="film.id" :film="film" />
            </div>
        </div>
    </section>
</template>

<script>
import Card from './Card.vue'; 

export default {
    name: 'CardContainer',
    components: {
        Card
    },
    props: {
        filmsArray: Array
    }
}
</script>

<style lang="scss" scoped>

</style>