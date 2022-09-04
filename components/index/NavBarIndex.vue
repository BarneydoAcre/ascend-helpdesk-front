<template>
    <div class="navbar">
        <v-card class="card">
            <v-card-text class="d-flex justify-space-between align-center">
                <div>
                    <v-btn to="/products/">
                        Produtos
                    </v-btn>
                    <v-btn to="/services/">
                        Serviços
                    </v-btn>
                    <v-btn to="/team/">
                        Equipe
                    </v-btn>
                </div>
                <div class="navbar-logo">
                    <img src="/logo.svg">
                </div>
            </v-card-text>
        </v-card>
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
.card {
}
.navbar-logo img {
    height: 8vh;
    border-radius: 1vh;
}
</style>