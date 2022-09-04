<template>
    <div class="navbar">
        <div class="navbar-background">
            <NuxtLink to="/products/" class="navbar-products navbar-button">Produtos</NuxtLink>
            <NuxtLink to="/services/" class="navbar-services navbar-button">Serviços</NuxtLink>
            <NuxtLink to="/team/" class="navbar-team navbar-button">Equipe</NuxtLink>
            <div class="navbar-logo">
                <img src="/logo.svg">
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'NavBarIndex',

}
</script>

<style>
.navbar {
    grid-area: navbar;
    
    padding: 8vh 6vw 3vh 6vw;
}
.navbar-background {
    display: grid;
    grid-template-columns: 13% 13% 13% 46%;
    grid-template-rows: 100%;
    grid-template-areas: 'products services team logo';
    grid-column-gap: 1vw;

    height: 100%;
}
.navbar-products {
    grid-area: products;
}
.navbar-services {
    grid-area: services;
}
.navbar-team {
    grid-area: team;
}
.navbar-logo {
    grid-area: logo;
    
    margin-left: auto;
}
.navbar-logo img {
    height: 100%;
    border-radius: 1vh;
}
.navbar-button {
    display: flex;
    justify-content: center;
    align-items: center;

    background: var(--baseColor);
    box-shadow: 1px 1px 15px -5px var(--lightColor);
    height: 80%;
    border-radius: 1vh;
    transition: background 1s;
}

.navbar-button:hover {
    background: var(--lightColor); 
}
</style>