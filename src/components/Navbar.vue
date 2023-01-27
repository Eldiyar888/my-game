<template>
    <div class="navbar">
        <div class="logo"><h2>Jeopardy</h2></div>
        <div class="navbar__btns">
            <!-- <router-link class="navbar-menu main" to="/auth">Главная</router-link> | -->
            <router-link class="navbar-menu game" to="/game"> Играть </router-link>
            <router-link class="navbar-menu game" to="/test"> test </router-link>
        </div>
    </div>
</template>

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
</style>