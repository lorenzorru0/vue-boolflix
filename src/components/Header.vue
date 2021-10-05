<template>
    <header>
        <div class="container d-flex align-items-center justify-content-between">
            <div>
                <img src="../assets/img/netflix.png" alt="Logo netflix">
            </div>
            <div>
                <input type="text" name="search" id="search" v-model="stringSearch">
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
    padding: 10px;

    img {
        width: 6.25rem;
    }

    input {
        text-align: center;
    }
}
</style>