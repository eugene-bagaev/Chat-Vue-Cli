<template>
    <v-app id="inspire" v-if="displayBody">
        <v-content>
            <v-container fluid fill-height>
                <v-layout align-center justify-center>
                    <v-flex xs12 sm8 md4>
                        <v-card class="elevation-12">
                            <v-toolbar dark color="primary">
                                <v-toolbar-title>Login</v-toolbar-title>
                                <v-spacer></v-spacer>
                                <v-tooltip bottom>
                                    <v-btn
                                            slot="activator"
                                            @click="signup"
                                            color="primary"
                                            target="_blank">
                                        Sign up
                                    </v-btn>
                                    <span>Need an account? Sign up</span>
                                </v-tooltip>
                            </v-toolbar>
                            <v-card-text>
                                <v-form ref="form">
                                    <v-text-field prepend-icon="person"
                                                  name="login"
                                                  label="Login"
                                                  v-model="login"
                                                  :rules="loginRules"
                                                  type="text"
                                                  required></v-text-field>
                                    <v-text-field id="password"
                                                  prepend-icon="lock"
                                                  v-model="password"
                                                  name="password"
                                                  required
                                                  :rules="passwordRules"
                                                  label="Password"
                                                  type="password"></v-text-field>
                                </v-form>
                            </v-card-text>
                            <v-card-actions>

                                <v-checkbox label="Remember me" v-model="remember"></v-checkbox>
                                <v-btn color="primary" @click="submit">Login</v-btn>

                            </v-card-actions>
                        </v-card>
                    </v-flex>
                </v-layout>
            </v-container>
        </v-content>
        <v-snackbar
                v-model="popupVisible"
                :color="popupColor"
                :top="popupTop"
                :timeout="popupDelay"
                :vertical="popupMode === 'vertical'">
            {{ popupText }}
            <v-btn dark flat @click="popupVisible = false">
                Close
            </v-btn>
        </v-snackbar>
    </v-app>
</template>

<script>
    export default {
        name: 'Login',
        data: () => ({
            login: '',
            password: '',
            popupText: 'Hello',
            popupColor: 'success',
            popupDelay: 6000,
            popupVisible : false,
            popupTop : true,
            popupMode: 'normal',
            drawer: null,
            remember: false,
            displayBody: true,
            passwordRules: [
                p => !!p || 'Password is required.',
                p => !(p.length <= 2) || 'Password must be longer than 2 characters.'
            ],
            loginRules: [
                v => !!v || 'Login is required.',
                v => !(v.length <= 2) || 'Login must be longer than 2 characters.'
            ],
            isError: false
        }),
        props: {
            source: String
        },
        methods: {
            submit() {
                if (this.$refs.form.validate()) {
                    let loginInfo = {
                        login       : this.login,
                        password    : this.password
                    };
                     let serverData = {
                         type : 'login',
                         data : JSON.stringify(loginInfo)
                     };

                    this.$socket.send(JSON.stringify(serverData));
                } else {
                    this.showToast('Please check the entered data in the form', 'error');
                }
            },
            loginLogic(serverData) {
                if (!serverData.hasAccess) {
                    this.loginUserError(serverData);
                } else if (serverData.hasAccess && serverData) {
                    this.loginSuccess(serverData);
                }
            },
            loginSuccess(serverData) {
                this.showToast(serverData.status, 'success');
                this.remember ? this.startSession(serverData) : this.saveUserSessionData(serverData);
                setTimeout(() => this.$emit('login', true), 1500);
            },
            loginUserError(serverData) {
                this.showToast(serverData.status, 'error');
            },
            showToast(text, color) {
                this.popupText      = text;
                this.popupColor     = color;
                this.popupVisible   = true;
            },
            startSession(serverData) {
                this.$session.start();
                this.$session.set('token',  serverData.key);
                this.$session.set('user',   serverData.username);
                this.$session.set('id',     serverData.id);
            },
            saveUserSessionData(serverData) {
                this.$session.start();
                this.$session.set('user',   serverData.username);
                this.$session.set('id',     serverData.id);
            },
            signup() {
                this.$emit('signup', true);
            }
        },
        created: function() {
            this.$options.sockets.onmessage = function (data) {
                this.loginLogic(JSON.parse(data.data));
            }
        },
        beforeMount() {
            if (this.$session.exists()) {
                this.displayBody = false;
                setTimeout(() => this.$emit('login', true), 0);
            } else {
                this.$emit('signin', true);
            }
        },
    }
</script>

<style scoped>
</style>