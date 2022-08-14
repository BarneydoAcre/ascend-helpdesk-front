<template>
    <v-app>
        <v-navigation-drawer mini-variant expand-on-hover fixed app>
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

            <v-list dense nav>
                <v-list-item v-for="item in items" :key="item.title" link>
                    <v-list-item-icon>
                        <v-icon>{{ item.icon }}</v-icon>
                    </v-list-item-icon>
                    <v-list-item-content>
                        <v-list-item-title>{{ item.title }}</v-list-item-title>
                    </v-list-item-content>
                </v-list-item>
            </v-list>
            
            <template v-slot:append>
                <v-list dense nav>
                    <v-list-item @click="logout" link>
                        <v-list-item-icon>
                            <v-icon>mdi-logout</v-icon>
                        </v-list-item-icon>
                        <v-list-item-content>
                            <v-list-item-title>Sair</v-list-item-title>
                        </v-list-item-content>
                    </v-list-item>
                </v-list>
            </template>
        </v-navigation-drawer>
        <v-app-bar fixed app>
            <v-row>
                <v-col cols="12">
                    <v-btn @click="clipped = false" class="col-2" elevation="4">
                        Produtos
                    </v-btn>
                    <v-btn to="/services/" class="col-2" elevation="4">
                        Serviços
                    </v-btn>
                    <v-btn to="/team/" class="col-2" elevation="4">
                        Equipe
                    </v-btn>
                </v-col>
            </v-row>
            <v-spacer></v-spacer>
                <v-btn-toggle>
                    <!-- ThemeLightDark -->
                    <v-btn v-if="$vuetify.theme.dark == true" @click="$vuetify.theme.dark = false">
                        <v-icon>mdi-theme-light-dark</v-icon>
                    </v-btn>
                    <v-btn v-else @click="$vuetify.theme.dark = true">
                        <v-icon>mdi-theme-light-dark</v-icon>
                    </v-btn>
                </v-btn-toggle>
        </v-app-bar>
    </v-app>
</template>

<script>
export default {
    name: 'dashboard',
    components: {},
    data () {
        return {
            items: [
                { title: 'Dashboard', icon: 'mdi-view-dashboard' },
                { title: 'Pastas', icon: 'mdi-folder' },
                { title: 'Ajuda', icon: 'mdi-help-box' },
            ],
            statusLogin: false,
        }
    },
    methods: {
        logout () {
            window.sessionStorage.setItem('access', '')
            window.sessionStorage.setItem('refresh', '')
            window.sessionStorage.setItem('username', '')
            window.sessionStorage.setItem('email', '')
            this.statusLogin = false
            this.verifyLogin()
        },
        async verifyLogin () {
            let token = {
                token: window.sessionStorage.getItem('refresh')
            }
            const req = await fetch(process.env.HOST_BACK+'/auth/jwt/verify/', {
                method: 'POST',
                body: JSON.stringify(token),
                headers: {'Content-Type': 'application/json'}
            })
            const res = await req
            
            if (req.status == 200) {
                this.statusLogin = true
            }else {
                this.statusLogin = false
                window.location.href = process.env.HOST_FRONT
            }
        },
    },
    created () {
        this.$vuetify.theme.dark = true
    },
    mounted () {
        this.verifyLogin ()
    }
}
</script>

<style>
</style>