<template>
    <header>
        <div class="container-fluid d-flex align-items-center justify-content-between">
            <div>
                <img src="../assets/img/netflix.png" alt="Logo netflix">
            </div>
            <div>
                <input type="text" name="search" id="search" v-model="stringSearch" placeholder="Search movies and Tv series">
                <button @click="$emit('stringSearch', stringSearch)">SEARCH</button>
            </div>
        </div>
    </header>
</template>

<script>
export default {
    name: 'Header',
    data() {
        return {
            stringSearch: ''
        }
    }
}
</script>

<style lang="scss" scoped>
header {
    background-color: rgb(20, 20, 20);
    position: fixed;
    top: 0;
    width: 100%;
    z-index: 9;
    padding: .625rem;
    box-shadow: 0 .25rem .9375rem -0.125rem #333;

    img {
        width: 6.25rem;
    }

    input, 
    button {
        margin-right: 1.25rem;
        padding: .3125rem .625rem;
        border: none;
        border: .0625rem solid #fff;
        background-color: inherit;
        color: #fff;
    }

    input {
        width: 18.75rem;
    }

    button {
        margin-right: 0;
        transition: transform 1s;
    }

    button:hover {
        transform: scale(1.2);
    }
}
</style>