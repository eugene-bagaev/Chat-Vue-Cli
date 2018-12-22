<template>
    <v-app id="inspire">
        <v-container grid-list-xl>

            <v-layout align-space-around justify-start column class="float-right message-width">
                <v-flex>
                    <v-card dark color="primary">
                        <v-card-text>one</v-card-text>
                    </v-card>
                </v-flex>
            </v-layout>

            <v-layout align-space-around justify-start column class="message-width right-100">
                <v-flex>
                    <v-card dark color="secondary">
                        <v-card-text class="text-lg-right">two</v-card-text>
                    </v-card>
                </v-flex>
            </v-layout>

            <v-layout align-space-around justify-start column class="float-right message-width">
                <v-flex>
                    <v-card dark color="primary">
                        <v-card-text>one</v-card-text>
                    </v-card>
                </v-flex>
            </v-layout>

            <v-layout align-space-around justify-start column class="message-width right-100">
                <v-flex>
                    <v-card dark color="secondary">
                        <v-card-text class="text-lg-right">two</v-card-text>
                    </v-card>
                </v-flex>
            </v-layout>

            <v-layout align-space-around justify-start column class="float-right message-width">
                <v-flex class="text-lg-right">
                    <v-card dark color="primary">
                        <v-card-text>one</v-card-text>
                    </v-card>
                </v-flex>
            </v-layout>
        </v-container>
    </v-app>

</template>

<script>
    export default {
        data () {
            return {
                message: ''
            }
        },
        name: "ChatContainer",
        methods: {
            getMessages() {
                var messages = [{
                    text: 'hello',
                    sender: 'Eugene'
                },
                    {
                        text: 'hello',
                        sender: 'Vitya'
                    }
                ];
                return messages;
            },
        }
    }
</script>

<style scoped>
    .float-right {
        float: right;
    }
    .message-width {
        width: 60%;
    }
    .right-100 {
        right: 100%;
    }


</style>