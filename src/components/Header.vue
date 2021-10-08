<template>
    <header>
        <div class="container-fluid d-flex align-items-center justify-content-between">
            <div class="d-flex align-items-center">
                <img src="../assets/img/boolflix.png" alt="Logo netflix">
                <!-- Header nav part -->
                <nav>
                    <ul class="d-flex">
                        <li><a href="#moviesDiv">Pupular Movies</a></li>
                        <li><a href="#tvsDiv">Popular Tv Series</a></li>
                        <template v-if="clickSearch != ''">
                            <li><a href="#moviesDivSearched">Movies searched</a></li>
                            <li><a href="#tvsDivSearched">Tv Series searched</a></li>
                        </template> 
                    </ul>
                </nav>
            </div>
            <div>
                <!-- Header search part -->
                <form>
                    <input type="text" name="search" id="search" v-model="stringSearch" placeholder="Search movies and Tv series">
                    <button @click.prevent="clickSearch = 'click', $emit('stringSearch', stringSearch)">SEARCH</button>
                    <button @click.prevent="clickSearch = '', stringSearch = '', $emit('stringSearch', stringSearch)">RESET</button>
                </form>
            </div>
        </div>
    </header>
</template>

<script>
export default {
    name: 'Header',
    data() {
        return {
            stringSearch: '',
            stringReset: 'reset',
            clickSearch: ''
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
    padding: .625rem 0;
    box-shadow: 0 .25rem .9375rem -0.125rem #333;

    img {
        width: 12.5rem;
    }

    nav {


        ul {
            list-style: none;
            margin-bottom: 0;
            padding-left: .625rem;

            li {
                
                a {
                    padding: .625rem;
                    text-decoration: none;
                    color: #fff;
                    transition: box-shadow 0.5s;

                    &:hover {
                        box-shadow: 0 0 .25rem #fff;
                    }
                }

                
            }
        }
    }

    input, 
    button {
        padding: .3125rem .625rem;
        border: none;
        border: .0625rem solid #fff;
        background-color: inherit;
        color: #fff;
    }

    input {
        max-width: 18.75rem;
    }

    button {
        margin-left: 1.25rem;
        transition: transform 1s;
    }

    button:hover {
        transform: scale(1.2);
    }
}
</style>