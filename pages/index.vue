<template>
    <div id="home-login">
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
:root {
    --textColor: #001f3f;
    --textColorShadow: #002d59;
    --textColorOpossite: #cad5ef;
    --baseColor: #ffffff;
    --lightColor: #a6a6a6;   
    --darkColor: #5d5d5d;
    --color4: #00e2ea;
    --color5: #2ee89d;
    --confirm: #079405;
    --confirm-hover: #067304;
    --alert: #9b8e00;
    --error: #f72222;
}
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    font-family: 'Poppins', sans-serif;
    text-decoration: unset;
    color: var(--textColor);
}
body {
    background: var(--baseColor);
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
