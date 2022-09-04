<template>
    <div id="home-login" style="background-color: var(--baseColor)">
        <NavBarIndex class="navbar" />
        <ContentIndex class="content" />
        <LoginRegister class="login-register" />
    </div>
</template>

<script>
import NavBarIndex from '../components/index/NavBarIndex.vue'
import ContentIndex from '../components/index/ContentIndex.vue'
import LoginRegister from '../components/index/LoginRegister.vue'
export default {
    name: "IndexPage",
    components: {
        NavBarIndex,
        ContentIndex,
        LoginRegister
    },
    methods: {
    },
    mounted() {
        window.location.href = "/#login-form";
    },
}
</script>

<style>
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    font-family: 'Poppins', sans-serif;
    text-decoration: unset;
}
body {
}
input {
    padding:.2vh;
}
input:focus-visible {
    outline: unset;
}
#home-login {
    display: grid;
    grid-template-columns: 60vw 40vw;
    grid-template-rows: 20vh 80vh;
    grid-template-areas: 'navbar navbar'
                         'content login-register';
}
</style>
