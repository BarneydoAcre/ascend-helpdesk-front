<template>
    <div class="login-register">
        <v-card height="100%">

            <div class="login-register-background">
                <div v-if="statusLogin" class="login-register-button">
                    <a :href="$route.path+'#login-form'" @click="logout()" class="login">Sair</a>
                </div>
                <div v-else class="login-register-button">
                    <a :href="$route.path+'#login-form'" class="login">Entre</a>
                    <p>|</p>
                    <a :href="$route.path+'#register-form'" class="signup">Registre-se</a>
                </div>


                <form id="login-form" class="modal">
                    <p class="form-title">Bem vindo ao Helpdesk</p>
                    <div class="input-field">
                        <label for="">Email</label>
                        <input type="email" v-model="formLogin.email">
                    </div>
                    <div class="input-field">
                        <div class="input-field-pass-label">
                            <label>Senha</label>
                            <a href="/accounts/restore-password/">Esqueceu sua senha?</a>
                        </div>
                        <input type="password" v-model="formLogin.password">
                    </div>
                    <div class="input-field" style="flex-direction: row; justify-content: flex-start; align-items: center;">
                        <label class="switch">
                            <input type="checkbox">
                            <span class="slider"></span>
                        </label>
                        <label for="">Lembrar de mim?</label>
                    </div>
                    <v-btn @click="login($event)">Entrar</v-btn>
                </form>

                <div id="register-form" class="modal">
                    <p class="form-title">Registre-se aqui!</p>
                    <v-form dense v-model="valid" ref="form" lazy-validation class="pl-10 pr-10">
                        <v-row dense>
                            <v-col cols="12">
                                <v-text-field dense label="Email" v-model="formRegister.email"></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field dense label="Primeiro Nome" v-model="formRegister.first_name"></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field dense label="Último Nome" v-model="formRegister.last_name"></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field dense label="Telefone ou Celular" v-model="formRegister.phone_number"></v-text-field>
                            </v-col>
                            <v-col cols="12" class="d-flex justify-center">
                                <v-btn class="button" @click="register">Registrar</v-btn>
                            </v-col>
                        </v-row>
                    </v-form>
                    <v-alert v-if="messageRegister" dismissible color="green">{{ messageRegister }}</v-alert>
                </div>


                <form id="company-form" class="modal">
                    <p class="form-title" style="text-align: center;">Escolha sua empresa!</p>
                    <div class="select-company">
                        <v-btn v-for="d,i in companys" :key="i" :href="'/'+d.slug+'/dashboard/'" @click="setCompany(d.company_id)">{{ d.company}}</v-btn>
                    </div>
                </form>
                
                
            </div>
        </v-card>
    </div>
</template>

<script>
export default {
    name: "LoginRegister",
    components: {
    },
    data () {
        return {
            valid: false,
            messageRegister: null,
            companys: [],
            statusLogin: false,
            formLogin: {
                email: '',
                password: '',
            },
            formRegister: {
                email: '',
                first_name: '',
                last_name: '',
                phone_number: '',
            },
        }
    },
    methods: {
        reset () {
            this.$refs.form.reset()
        },
        async verifyLogin () {
            let token = {
                token: localStorage.getItem('refresh')
            }
            const req = await fetch(process.env.HOST_BACK+'/auth/jwt/verify/', {
                method: 'POST',
                body: JSON.stringify(token),
                headers: {'Content-Type': 'application/json'}
            })
            const res = await req
            
            if (req.status == 200) {
                this.getCompany()
                this.statusLogin = true
                window.location.href = "/#company-form"
            }
        },
        
        async login(e) {
            e.preventDefault()
            // const req = await fetch(process.env.HOST_BACK+'/jwt/create/', {
            const req = await fetch(process.env.HOST_BACK+'/auth/login/', {
                method: 'POST',
                body: JSON.stringify(formLogin),
                headers: {'Content-Type': 'application/json'}
            })
            const res = await req.json()
            if (req.status == '200') {
                localStorage.setItem('access', res.login_token.access)
                localStorage.setItem('refresh', res.login_token.refresh)
                localStorage.setItem('user_id', res.user_id)              
                localStorage.setItem('username', res.username)
                localStorage.setItem('email', res.email)
                
                this.getCompany()
                this.verifyLogin()
                window.location.href = ('/#company-form')
            }else {
                this.validLogin = true;
                this.password = ''
            }
        },
        async register () {
            const req = await fetch(process.env.HOST_BACK+'/auth/register/', {
                method: 'POST',
                body: JSON.stringify(this.formRegister),
                headers: {'Content-Type': 'application/json'},
            })
            if (req.status == 200) {
                this.messageRegister = "Seus dados foram enviados!"
                this.reset()
            }
        },
        logout () {
            localStorage.setItem('access', '')
            localStorage.setItem('refresh', '')
            localStorage.setItem('user_id', '')
            localStorage.setItem('username', '')
            localStorage.setItem('email', '')
            localStorage.setItem('company', '')
            this.statusLogin = false
        },
        async getCompany () {
            let email = localStorage.getItem('email')
            const req = await fetch(process.env.HOST_BACK+'/getCompany/?email='+email+'&key=8168', {
                method: 'GET'
            })
            const res = await req.json()
            this.companys = res
        },
        setCompany (id) {
            localStorage.setItem('company', id)
        }
    },
    mounted () {
        this.verifyLogin()
    }
}
</script>

<style>
.modal {
    z-index: 99999;
    opacity:0;
    transition: opacity 200ms ease-in;
    pointer-events: none;
}
.modal:target {
  opacity: 1;
  pointer-events: auto;
}
.input-field {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;

    width: 80%;
    margin-bottom: 2vh;
}
.input-field input {
    width: 90%;
    margin: 0 5% 0 5%;
    border: unset;
    border-radius: .5vh;
}
.input-field label {
    font-size: 1.8vh;
    margin: 0 5% 0 5%;
} 
.input-field-pass-label label {
    margin: 0;
}
.input-field-pass-label a {
    color: rgb(218, 207, 0);
    font-weight: bold;
    font-size: 1.5vh;
}
.input-field-pass-label {
    display: flex;
    justify-content: space-between;
    align-items: center;

    width: 90%;
    margin: 0 5% 0 5%;
}
.input-field-pass-label label  {
    font-size: 1.9vh;
}
.login-register {
    grid-area: login-register;
    
    padding: 8vh 6vw 8vh 4vw;
}
.login-register-background {
    display: grid;
    grid-template-columns: 100%;
    grid-template-rows: 10% 90%;
    grid-template-areas: 'buttons'
                         'form';

    height: 100%;
    border-radius: 3vh;
    background: var(--baseColor);
    box-shadow: 1px 1px 15px -5px var(--lightColor);

}
.login-register-button {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 2vh;
}
.login-register-button a {
    border-radius: 3vh;
    padding: 1vh 1vw;
    font-size: 1.8vh;
    margin: 0 1vw;
}
.form-title {
    width: 75%;
    font-size: 18px;
    font-weight: bold;
    margin: 4vh 0 4vh 0;
}

#login-form {
    grid-area: form;

    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-bottom: 8vh;
}
/* INPUT CSS*/
.switch {
 font-size: 17px;
 position: relative;
 display: inline-block;
 width: 3.5em;
 height: 2em;
 margin: 0 5% 5% 10%;
}
/* Hide default HTML checkbox */
.switch input {
 opacity: 0;
 width: 0;
 height: 0;
}
/* The slider */
.slider {
 position: absolute;
 cursor: pointer;
 top: 0;
 left: 0;
 right: 0;
 bottom: 0;
 background-color: #ccc;
 transition: .4s;
 border-radius: 30px;
}

.slider:before {
 position: absolute;
 content: "";
 height: 1.4em;
 width: 1.4em;
 border-radius: 20px;
 left: 0.3em;
 bottom: 0.3em;
 background-color: white;
 transition: .4s;
}
input:checked + .slider {
 background-color: #2c007e;
}

input:focus + .slider {
 box-shadow: 0 0 1px #2c007e;
}

input:checked + .slider:before {
 transform: translateX(1.5em);
}
#register-form {
    grid-area: form;

    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
}
#company-form {
    grid-area: form;

    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
}
.select-company {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.select-company a {
    padding: 2vh;
    margin: 1vh;
    border: 2px solid var(--textColor);
}
</style>