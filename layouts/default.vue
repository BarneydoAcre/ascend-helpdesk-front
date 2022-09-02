<template>
    <v-app>
        <v-navigation-drawer v-if="statusLogin == 2" permanent mini-variant expand-on-hover fixed app>
            <v-list-item>
                <v-list-item-content>
                    <v-list-item-title class="text-h6">
                        Application
                    </v-list-item-title>
                    <v-list-item-subtitle>
                        subtext
                    </v-list-item-subtitle>
                </v-list-item-content>
            </v-list-item>

            <v-divider></v-divider>

            <v-list dense nav v-if="$route.params.company">
                <v-list-item v-for="item in items" :key="item.title" :disabled="item.disabled" :to="'/'+$route.params.company+'/'+item.link">
                    <v-list-item-icon>
                        <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content>
                        <v-list-item-title>{{ item.title }}</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
            
            <template v-slot:append>
                <v-list dense nav v-if="$route.params.company">
                    <v-list-item link :to="'/'+$route.params.company+'/settings/'">
                        <v-list-item-icon>
                            <v-icon>mdi-cog</v-icon>
                        </v-list-item-icon>
                        <v-list-item-content>
                            <v-list-item-title>Configurações</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
                <v-list dense nav>
                    <v-list-item @click="logout" link>
                        <v-list-item-icon>
                            <v-icon>mdi-logout</v-icon>
                        </v-list-item-icon>
                        <v-list-item-content>
                            <v-list-item-title>Sair</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                <!-- ThemeLightDark -->
                    <v-list-item v-if="$vuetify.theme.dark == true" @click="$vuetify.theme.dark = false">
                        <v-list-item-icon>
                            <v-icon>mdi-theme-light-dark</v-icon>
                        </v-list-item-icon>
                        <v-list-item-content>
                            <v-list-item-title>Modo Claro/Escuro</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                    <v-list-item v-else @click="$vuetify.theme.dark = true">
                        <v-list-item-icon>
                            <v-icon>mdi-theme-light-dark</v-icon>
                        </v-list-item-icon>
                        <v-list-item-content>
                            <v-list-item-title>Modo Claro/Escuro</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
            </template>
        </v-navigation-drawer>
        <v-main v-if="statusLogin == 2">
            <v-container>
                <Nuxt />
            </v-container>
        </v-main>
        <Nuxt v-if="statusLogin == 1"/>
        <v-speed-dial
        class="v-btn-fab"
        :transition="'scale-transition'">
            <template v-slot:activator>
                <BugReport></BugReport>
            </template>
        </v-speed-dial>
    </v-app>
</template>

<script>
import BugReport from '../components/default/BugReport.vue'
export default {
    name: "DefaultLayout",
    components: {
        BugReport,
    },
    data() {
        return {
            items: [
                { title: "Dashboard (Em Breve)", icon: "mdi-view-dashboard", link: "dashboard/", disabled: true },
                { title: "Comercial (Em Breve)", icon: "mdi-bullhorn", link: "business/", disabled: true },
                { title: "Administrativo (Em Breve)", icon: "mdi-hand-coin", link: "administrative/", disabled: true },
                { title: "Projetos (Em Breve)", icon: "mdi-note-text", link: "projects/", disabled: true },
                { title: "Suporte", icon: "mdi-face-agent", link: "suport/", disabled: false },
                { title: "Ajuda (Em Breve)", icon: "mdi-help-box", link: "help/", disabled: true },
            ],
            statusLogin: 0,
            clipped: false,
            drawer: false,
            fixed: false,
            miniVariant: false,
            right: true,
            rightDrawer: false,
            title: "Vuetify.js"
        };
    },
    mounted() {
        this.verifyLogin();
        // console.log(this.)
    },
    beforeCreate() {
        this.$vuetify.theme.isDark = false;
    },
    methods: {
        logout() {
            localStorage.setItem("access", "");
            localStorage.setItem("refresh", "");
            localStorage.setItem("user_id", "");
            localStorage.setItem("username", "");
            localStorage.setItem("email", "");
            localStorage.setItem("company", "");
            this.statusLogin = 1;
            this.verifyLogin();
        },
        async verifyLogin() {
            let token = {
                token: localStorage.getItem("refresh")
            };
            const req = await fetch(process.env.HOST_BACK + "/auth/jwt/verify/", {
                method: "POST",
                body: JSON.stringify(token),
                headers: { "Content-Type": "application/json" }
            });
            const res = await req;
            if (req.status == 200) {
                this.statusLogin = 2;
            }
            else {
                this.statusLogin = 1;
                if (this.$route.path != "/") {
                    window.location.href = process.env.HOST_FRONT;
                }
            }
        },
    },
}
</script>

<style>
.v-btn-fab {
    position: absolute;
    bottom: 0;
    right: 0;
    margin: 0 2vw 4vh 0;
}
</style>