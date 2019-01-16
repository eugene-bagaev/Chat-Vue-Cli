<template>
    <v-container fill-height fluid v-if="true">
        <v-card style="margin: auto; width: 700px">
            <v-toolbar flat color="green white--text">
                <v-toolbar-title>Create an Account</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-tooltip bottom>
                    <v-btn
                            slot="activator"
                            @click="signin"
                            color="success"
                            target="_blank"
                    >
                        Sign in
                    </v-btn>
                    <span>Already registered? Sign in</span>
                </v-tooltip>
            </v-toolbar>
            <v-form class="pl-2 pr-2" v-model="valid" ref="form" lazy-validation>
                <v-container grid-list-xl fluid>
                    <!-- First Name -->
                    <v-layout wrap>

                        <v-flex xs12 sm6>
                            <v-text-field
                                    label="First Name"
                                    v-model="firstName"
                                    :rules="[v => !!v || 'First Name is required.']"
                                    required
                            ></v-text-field>
                        </v-flex>

                        <v-flex xs12 sm6>
                            <v-text-field
                                    label="Last Name"
                                    v-model="lastName"
                                    :rules="[v => !!v || 'Last Name is required.']"
                                    required
                            ></v-text-field>
                        </v-flex>
                    </v-layout>

                    <v-layout wrap>
                        <v-flex xs12 sm6>
                            <v-text-field
                                    label="Email Address"
                                    v-model="email"
                                    :rules="emailRules"
                                    required
                            ></v-text-field>
                        </v-flex>

                        <v-flex xs12 sm6>
                            <v-text-field
                                    label="Login"
                                    v-model="login"
                                    :rules="[v => !!v || 'Login is required.']"
                                    required
                            ></v-text-field>
                        </v-flex>

                    </v-layout>
                    <v-layout wrap>

                        <v-flex xs12 sm6>
                            <v-text-field
                                    label="Password"
                                    v-model="password"
                                    type="password"
                                    :rules="passwordRules"
                                    required
                            ></v-text-field>
                        </v-flex>

                        <v-flex xs12 sm6>
                            <v-text-field
                                    label="Confirm Your Password"
                                    v-model="confirmPassword"
                                    :rules="validatePasswords()"
                                    type="password"
                                    required
                                    @keyup.enter="submit"
                            ></v-text-field>
                        </v-flex>

                    </v-layout>
                    <v-layout>
                        <v-checkbox label="Autologin after registration" color="success" v-model="autoLogin"></v-checkbox>

                        <v-btn
                                class="white--text"
                                color="green"
                                :loading="loading"
                                @click="submit"
                                :disabled="!valid">
                            Create Account
                        </v-btn>

                        <v-btn @click="clear">clear</v-btn>

                    </v-layout>
                </v-container>
            </v-form>
        </v-card>
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
    </v-container>
</template>

<script>
    export default {
        data: () => ({
            passwordRules: [
                p => !!p || 'Password is required.',
                p => !(p.length <= 2) || 'Password must be longer than 2 characters.'
            ],
            emailRules: [
                value => {
                    const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
                    return pattern.test(value) || 'Invalid e-mail address.'
                },
                v => !!v || ' email is required.'
            ],
            valid           : true,
            firstName       : '',
            lastName        : '',
            login           : '',
            email           : '',
            password        : '',
            confirmPassword : '',
            loading         : false,
            popupText       : 'Hello',
            popupColor      : 'success',
            popupDelay      : 6000,
            popupVisible    : false,
            popupTop        : true,
            autoLogin       : false,
            popupMode       : 'normal'
        }),
        methods: {
            validatePasswords() {
                return [p => p === this.password || 'Passwords must match.']
            },
            submit() {
                if (this.$refs.form.validate()) {
                    let regInfo = {
                        firstName   : this.firstName,
                        lastName    : this.lastName,
                        login       : this.login,
                        email       : this.email,
                        password    : this.password
                    };
                    let serverData = {
                        type : 'reg',
                        data : JSON.stringify(regInfo)
                    };
                    this.$socket.send(JSON.stringify(serverData));
                } else {
                    this.showToast('Please check the entered data in the form', 'error');
                }
            },
            registrationLogic(serverData) {
                if (!serverData.hasAccess) {
                    this.registrationError(serverData);
                } else if (serverData.hasAccess && serverData) {
                    this.registrationSuccess(serverData);
                }
            },
            registrationSuccess() {
                let successText = this.autoLogin ? 'Success. Now you will be login in Chat' : 'Success. Please login';
                this.showToast(successText, 'success');
                if (this.autoLogin) {
                    setTimeout(() => this.loginUserAfterRegistration(), 2000);
                } else {
                    setTimeout(() => this.$emit('signin', true), 2000);
                }
            },
            loginUserAfterRegistration() {
                this.$emit('login', true);
            },
            registrationError(serverData) {
                this.showToast(serverData.status, 'error');
            },
            showToast(text, color) {
                this.popupText = text;
                this.popupColor = color;
                this.popupVisible = true;
            },
            signin() {
                this.$emit('signin', true);
            },
            clear() {
                this.$refs.form.reset();
                this.firstName = '';
                this.lastName = '';
                this.email = '';
                this.password = '';
                this.confirmPassword = '';
            },
        },
        created: function() {
            this.$options.sockets.onmessage = function (data) {
                this.registrationLogic(JSON.parse(data.data));
            }
        }
    }
</script>

<style>
</style>