<template>
    <div class="navbar">
        <div class="logo"><h2>ИГРА</h2></div>
        <div class="navbar__btns">
            <router-link class="navbar-menu main" to="/">Главная</router-link> |
            <router-link class="navbar-menu game" to="/game"> Игра </router-link>
        </div>
    </div>
</template>

<script lang="ts">

</script>

<style scoped>
.navbar {
    height: 50px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #151b4b;
    color: #fff;
}

.logo {
    margin: 0 30px;
    color: #fff;
}

.navbar__btns {
    margin: 0 30px;
}

.navbar__btns a {
    text-decoration: none;
    color: #fff;
}

/* a.router-link {
    text-transform: uppercase;
} */

/* a.router-link-active, */
/* li.router-link-active>a {
    color: gold;
} */

/* .navbar-menu {
    text-transform: uppercase;
} */
</style>