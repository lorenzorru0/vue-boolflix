<template>
    <div>
        <ul>
            <li>{{film.title}}</li>
            <li>{{film.original_title}}</li>
            <li>{{film.original_language}}</li>
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

</style>