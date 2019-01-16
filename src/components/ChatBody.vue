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
            <v-list two-line subheader>

                <v-subheader inset>
                    Online users
                </v-subheader>

                <v-list-tile
                    v-for="item in users"
                    :key="item.title"
                    avatar
                    @click=""
                >
                    <v-list-tile-avatar>
                        <v-icon :class="tempAvatarClass">
                            {{ tempAvatar }}
                        </v-icon>
                    </v-list-tile-avatar>

                    <v-list-tile-content>
                        <v-list-tile-title>{{ item.username }}</v-list-tile-title>
                        <v-list-tile-sub-title>{{ item.name }}</v-list-tile-sub-title>
                    </v-list-tile-content>

                    <v-list-tile-action>
                    <v-btn
                        icon
                        ripple
                    >
                        <v-icon color="grey lighten-1">
                        info
                        </v-icon>
                    </v-btn>
                    </v-list-tile-action>
                </v-list-tile>


            </v-list>
        </v-navigation-drawer>

        <v-navigation-drawer
                persistent
                :mini-variant="miniVariant"
                :clipped="clipped"
                v-model="rightDrawer"
                enable-resize-watcher
                :right="true"
                :width="400"
                fixed
                app
        >
            <v-card>
                <v-img
                        src="https://cdn.vuetifyjs.com/images/lists/ali.png"
                        height="300px"
                >
                    <v-layout
                            column
                            fill-height
                    >
                        <v-card-title>
                            <v-btn dark icon @click="scrollToEnd">
                                <v-icon>chevron_left</v-icon>
                            </v-btn>

                            <v-spacer></v-spacer>

                            <v-btn dark icon class="mr-3">
                                <v-icon>edit</v-icon>
                            </v-btn>

                            <v-btn dark icon>
                                <v-icon>more_vert</v-icon>
                            </v-btn>
                        </v-card-title>

                        <v-spacer></v-spacer>

                        <v-card-title class="white--text pl-5 pt-5">
                            <div class="display-1 pl-5 pt-5">Ali Conners</div>
                        </v-card-title>
                    </v-layout>
                </v-img>

                <v-list two-line subheader>
                    <v-list-tile>
                        <v-list-tile-action>
                            <v-icon color="indigo">phone</v-icon>
                        </v-list-tile-action>

                        <v-list-tile-content>
                            <v-list-tile-title>(650) 555-1234</v-list-tile-title>
                            <v-list-tile-sub-title>Mobile</v-list-tile-sub-title>
                        </v-list-tile-content>

                        <v-list-tile-action>
                            <v-icon>chat</v-icon>
                        </v-list-tile-action>
                    </v-list-tile>

                    <v-list-tile>
                        <v-list-tile-action></v-list-tile-action>

                        <v-list-tile-content>
                            <v-list-tile-title>(323) 555-6789</v-list-tile-title>
                            <v-list-tile-sub-title>Work</v-list-tile-sub-title>
                        </v-list-tile-content>

                        <v-list-tile-action>
                            <v-icon>chat</v-icon>
                        </v-list-tile-action>
                    </v-list-tile>

                    <v-divider inset></v-divider>

                    <v-list-tile>
                        <v-list-tile-action>
                            <v-icon color="indigo">mail</v-icon>
                        </v-list-tile-action>

                        <v-list-tile-content>
                            <v-list-tile-title>aliconnors@example.com</v-list-tile-title>
                            <v-list-tile-sub-title>Personal</v-list-tile-sub-title>
                        </v-list-tile-content>
                    </v-list-tile>

                    <v-list-tile>
                        <v-list-tile-action></v-list-tile-action>

                        <v-list-tile-content>
                            <v-list-tile-title>ali_connors@example.com</v-list-tile-title>
                            <v-list-tile-sub-title>Work</v-list-tile-sub-title>
                        </v-list-tile-content>
                    </v-list-tile>

                    <v-divider inset></v-divider>

                    <v-list-tile>
                        <v-list-tile-action>
                            <v-icon color="indigo">location_on</v-icon>
                        </v-list-tile-action>

                        <v-list-tile-content>
                            <v-list-tile-title>1400 Main Street</v-list-tile-title>
                            <v-list-tile-sub-title>Orlando, FL 79938</v-list-tile-sub-title>
                        </v-list-tile-content>
                    </v-list-tile>
                </v-list>
            </v-card>
        </v-navigation-drawer>

        <v-toolbar
                absolute
                color="orange lighten-1"
                ref="toolbarItem"
                dark
                fixed
                scroll-target="#scrolling-techniques">
            <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
            <v-toolbar-title>
                Title
            </v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn icon>
                <v-icon>search</v-icon>
            </v-btn>
            <v-btn icon @click="isDark = !isDark">
                <v-icon>invert_colors</v-icon>
            </v-btn>
            <v-btn icon @click="scrollToEnd">
                <v-icon>favorite</v-icon>
            </v-btn>
            <v-btn icon @click="rightDrawer = !rightDrawer">
                <v-icon>info</v-icon>
            </v-btn>
            <v-btn icon @click="logout">
                <v-icon>exit_to_app</v-icon>
            </v-btn>
        </v-toolbar>

        <v-layout>
            <v-flex xs12 sm6 offset-sm3>
                <div id="scrollingContainer"
                     class="scroll-y"
                     :style="'max-height: ' + getInnerHeight + 'px; margin-top:64px;margin-bottom:56px;'">
                    <v-container :style="'max-height: ' + getInnerHeight + 'px'">
                        <ul>
                            <li v-for="(item, i) in messages" :key="i" :class="item.userlogin == user ? 'me' : 'him'">
                                <div style="font-weight:bold">{{ item.username }}</div>
                                {{item.message}}
                            </li>
                        </ul>
                    </v-container>
                </div>
            </v-flex>
        </v-layout>

        <v-bottom-nav
                :value="true"
                fixed>
            <v-spacer></v-spacer>
            <v-btn  color="teal"
                    flat
                    value="recent">
                <span>Recent</span>
                <v-icon>history</v-icon>
            </v-btn>

            <v-text-field   label="Message"
                            v-model="msg"
                            @keyup.enter="sendMsg"
                            single-line
                            :rules="msgRules"
            ></v-text-field>

            <v-btn  @click="sendMsg"
                    medium
                    color="primary"
                    flat
                    value="send">
                <span>Send</span>
                <v-icon>send</v-icon>
            </v-btn>

            <v-spacer></v-spacer>
        </v-bottom-nav>

        <v-snackbar
                v-model="popupVisible"
                :color="popupColor"
                :top="popupTop"
                :left="true"
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
        name: 'ChatBody',
        data () {
            return {
                user        : '',
                id          : '',
                messages    : '',
                users       : '',
                message     : this.msg,
                isDark      : false,
                clipped     : true,
                drawer      : false,
                msg         : '',
                fixed       : false,
                icon        : 'bubble_chart',
                items2       : [{
                    icon: 'bubble_chart',
                    title: 'Inspire'},
                ],
                miniVariant : false,
                right       : true,
                loading     : false,
                rightDrawer : false,
                title       : '',
                popupVisible: false,
                popupColor  : 'error',
                popupTop    : true,
                popupDelay  : 0,
                popupMode   : 'normal',
                popupText   : 'Server Connection Closed',
                msgRules    : [
                    p => !!p || 'Message is required.',
                    p => !(p.length <= 0) || 'Message must be longer than 2 characters.'
                ],
                tempAvatar: 'account_circle',
                tempAvatarClass: 'grey lighten-1 white--text',
                items: [
                    { icon: 'folder', iconClass: 'grey lighten-1 white--text', title: 'Photos', subtitle: 'Jan 9, 2014' },
                    { icon: 'folder', iconClass: 'grey lighten-1 white--text', title: 'Recipes', subtitle: 'Jan 17, 2014' },
                    { icon: 'folder', iconClass: 'grey lighten-1 white--text', title: 'Work', subtitle: 'Jan 28, 2014' }
                ],
            }
        },
        methods: {
            chatLogic(serverData) {
                if (serverData.type === "newMsg") {
                    this.messages.push(serverData.message);
                    
                    this.scrollToEnd();
                } else if (serverData.type === "info") {
                    this.scrollToEnd();
                } else if (serverData.type === "users") {
                    console.log((serverData.users));
                    this.users = serverData.users;
                } else {
                    this.messages = serverData.messages;
                    this.scrollToEnd();
                }
            },
            sendMsg() {
                if (!this.msg) return;
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
                var container = this.$el.querySelector("#scrollingContainer");
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
            initUser() {
                let initUserData = {
                    id          : this.id,
                    username    : this.user,
                    token       : this.$cookies.get('token')
                };
                console.log('Path: ', location.href);
                let initData = {
                    type : 'inituser',
                    data : JSON.stringify(initUserData)
                };
                this.$socket.send(JSON.stringify(initData));
            },
            logout() {
                this.$cookies.remove('token');
                this.$cookies.remove('user');
                this.$cookies.remove('id');
                this.$emit('logout', true);
            },
            showToast(text, color) {
                this.popupText      = text;
                this.popupColor     = color;
                this.popupVisible   = true;
            },
            errorLogic(event) {
                var message = event.wasClean ? 'Соединение закрыто чисто. ' : 'Обрыв соединения. ';
                console.log(message += 'Код: ' + event.code + ' причина: ' + event.reason);
                this.popupVisible = true;
            }
        },
        computed: {
            getInnerHeight() {
                // 120 = toolbar height + bottom bar height
                var newHeight = window.innerHeight - 120;
                return newHeight;
            }
        },
        created() {
            this.user   = this.$cookies.get("user");
            this.id     = this.$cookies.get("id");
            this.requestLatestMessages();
            this.$options.sockets.onmessage = function (data) {
                this.chatLogic(JSON.parse(data.data));
            };
            this.$options.sockets.onclose = function (event) {
                this.errorLogic(event);
            };
            this.initUser();
        }
    }
</script>

<style>
    ::-webkit-scrollbar {
        width: 3px;
        border-radius: 1px;
    }

    /* Track */
    ::-webkit-scrollbar-track {
        background: #f1f1f1;
    }

    /* Handle */
    ::-webkit-scrollbar-thumb {
        background: #ffa726;
    }

    /* Handle on hover */
    ::-webkit-scrollbar-thumb:hover {
        background: #a96521;
        width: 10px;
    }
    ul{
        list-style: none;
        margin: 0;
        padding: 0;
    }
    ul li{
        display:inline-block;
        clear: both;
        padding: 10px;
        padding-left: 30px;
        padding-right: 30px;
        border-radius: 10px;
        margin-bottom: 2px;
    }
    .him{
        background: #eee;
        float: left;
        margin-left:8px;
    }
    .me{
        float: right;
        background: #0084ff;
        color: #fff;
        margin-right:8px;
    }
    .him + .me{
        border-bottom-right-radius: 5px;
    }
    .me + .me{
        border-top-right-radius: 5px;
        border-bottom-right-radius: 5px;
    }
    .me:last-of-type {
        border-bottom-right-radius: 10px;
    }

</style>