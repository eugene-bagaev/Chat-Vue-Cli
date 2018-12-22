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
                    extended
                    flat>
                <v-toolbar-side-icon @click.stop="drawer = !drawer"></v-toolbar-side-icon>
            </v-toolbar>

            <v-layout row pb-2>
                <v-flex xs8 offset-xs2>
                    <v-card class="card--flex-toolbar">
                        <v-toolbar card prominent>
                            <v-toolbar-title class="body-2 grey--text">Title</v-toolbar-title>

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

                            <v-btn icon>
                                <v-icon>more_vert</v-icon>
                            </v-btn>
                        </v-toolbar>

                        <v-divider></v-divider>

                        <v-card-text>
                            <ChatBody/>
                        </v-card-text>
                    </v-card>
                </v-flex>
            </v-layout>
        </v-card>
    </v-app>
</template>

<script>
    import ChatBody from '../components/ChatBody'

    export default {
        components: {
            ChatBody
        },
        data () {
            return {
                isDark: false,
                clipped: true,
                drawer: true,
                fixed: false,
                items: [{
                    icon: 'bubble_chart',
                    title: 'Inspire'},
                ],
                miniVariant: false,
                right: true,
                rightDrawer: false,
                title: 'Vuetify.js'
            }
        },
        computed : {
        },
        methods: {
            isMobileDevice() {

            }
        }

    }
</script>
<style>
    .card--flex-toolbar {
        margin-top: -64px;
    }
</style>