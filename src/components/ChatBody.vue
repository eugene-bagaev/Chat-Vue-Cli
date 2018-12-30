<template>

    <v-app :dark="isDark">
        <v-navigation-drawer
                persistent
                :mini-variant="miniVariant"
                :clipped="clipped"
                v-model="drawer"
                enable-resize-watcher
                fixed
                app>
            <v-list>
                <v-list-tile
                        value="true"
                        v-for="(item, i) in items"
                        :key="i">

                    <v-list-tile-action>
                        <v-icon v-html="item.icon"></v-icon>
                    </v-list-tile-action>

                    <v-list-tile-content>
                        <v-list-tile-title v-text="item.title"></v-list-tile-title>
                    </v-list-tile-content>

                </v-list-tile>
            </v-list>
        </v-navigation-drawer>
        <v-card flat xs>
            <v-toolbar
                    color="primary"
                    dark

                    flat>
                <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
            </v-toolbar>
            <v-layout row pb-2>
                <v-flex xs8 offset-xs2>
                    <v-card class="card--flex-toolbar">
                        <v-toolbar card prominent>
                            <v-toolbar-title class="body-2 grey--text">{{ user }}</v-toolbar-title>
                            <v-spacer></v-spacer>
                            <v-btn icon @click="isDark = !isDark">
                                <v-icon>invert_colors</v-icon>
                            </v-btn>
                            <v-btn icon>
                                <v-icon>search</v-icon>
                            </v-btn>
                            <v-btn icon>
                                <v-icon>apps</v-icon>
                            </v-btn>
                            <v-tooltip bottom>
                                <v-btn  slot="activator"
                                        icon
                                        @click="logout"
                                        large
                                        target="_blank">
                                    <v-icon >exit_to_app</v-icon>
                                </v-btn>
                                <span>Logout</span>
                            </v-tooltip>

                        </v-toolbar>
                        <v-divider></v-divider>
                        <v-card-actions id="msgContainer" :style="'overflow:auto;max-height:'+getInnerHeight+'px;min-height:'+getInnerHeight+'px;'">

                            <v-app id="inspire"  style="width: 100%;">
                                <v-container grid-list-xl>

                                    <v-layout align-space-around
                                              v-for="(item, i) in messages"
                                              :key="i"
                                              justify-start column
                                              :class="item.userlogin == user ? 'float-right message-width' : 'message-width right-100'">

                                        <v-flex :class="item.userlogin == user ? 'text-lg-right' : ''">

                                            <v-card dark :color="item.userlogin == user ? 'primary' : 'secondary'">
                                                <v-card-text >{{item.message}}</v-card-text>
                                            </v-card>

                                        </v-flex>
                                    </v-layout>

                                </v-container>
                            </v-app>

                        </v-card-actions>
                    </v-card>
                </v-flex>
            </v-layout>
        </v-card>
        <v-footer class="pa-3" style="height: 80px; background: white">

            <v-spacer></v-spacer>

            <v-text-field
                    label="Messge"
                    v-model="msg"
                    @keyup.enter="sendMsg"
                    single-line
            ></v-text-field>

            <v-btn @click="sendMsg" medium color="primary">
                Send
                <v-icon right dark>send</v-icon>
            </v-btn>

            <v-spacer></v-spacer>

        </v-footer>
    </v-app>
</template>

<script>
    export default {
        name: 'ChatBody',
        data () {
            return {
                user        : '',
                id          : '',
                messages    : '',
                message     : this.msg,
                isDark      : false,
                clipped     : true,
                drawer      : true,
                msg         : '',
                fixed       : false,
                items       : [{
                    icon: 'bubble_chart',
                    title: 'Inspire'},
                ],
                miniVariant : false,
                right       : true,
                loading     : false,
                rightDrawer : false,
                title       : ''
            }
        },
        methods: {
            chatLogic(serverData) {
                if (serverData.type === "newMsg") {
                    this.messages.push(serverData.message);
                    this.scrollToEnd();
                } else if (serverData.type === "info") {
                    this.scrollToEnd();
                } else {
                    this.messages = serverData.messages;
                    this.scrollToEnd();
                }
            },
            sendMsg() {
                let msgData = {
                    userId : this.id,
                    name : this.name,
                    msg: this.msg
                };
                let serverData = {
                    type : 'chat',
                    data : JSON.stringify({
                        dataType : 'saveMsgSendToUsers',
                        dataValues : JSON.stringify(msgData)
                    })
                };
                this.msg = '';
                this.$socket.send(JSON.stringify(serverData));
            },
            scrollToEnd: function() {
                var container = this.$el.querySelector("#msgContainer");
                container.scrollTop = container.scrollHeight;
            },
            requestLatestMessages() {
                let serverData = {
                    type : 'chat',
                    data : JSON.stringify({
                        dataType : 'MesReq',
                        dataValues: null
                    })
                };
                this.$socket.send(JSON.stringify(serverData));
            },
            logout() {
                this.$session.destroy();
                this.$emit('logout', true);
            }
        },
        computed: {
            getInnerHeight() {
                var newHeight = screen.height - 295;
                return newHeight;
            }
        },
        created() {
            this.user   = this.$session.get("user");
            this.id     = this.$session.get("id");
            this.requestLatestMessages();
            this.$options.sockets.onmessage = function (data) {
                this.chatLogic(JSON.parse(data.data));
            }
        }
    }
</script>

<style>
    .float-right {
        float: right;
    }
    .message-width {
        width: 60%;
    }
    .right-100 {
        right: 100%;
    }
    .application--wrap {
        max-height: 100%;
        min-height: 75vh;
    }
    .card--flex-toolbar {
        margin-top: -65px;
    }
    .container {
        padding: 10px;

    }
</style>