<template lang="pug">
    v-app
        v-app-bar(
            extended
            flat
            dark
            src="https://i.picsum.photos/id/885/1920/1080.jpg?blur=2&grayscale&hmac=kfH8ExnZO6NCpKokwT_LWHk9oLtmd-4cEctUdxJEuss"
        )
            template(
                v-slot:img="{ props }"
            )
                v-img(
                    v-bind="props"
                )
            .logo
                v-avatar(
                    tile
                )
                    v-img(
                        height="50px"
                        width="50px"
                        src="numzero.png"
                        contain
                    )
            v-toolbar-title YourProfile
            v-spacer
            .authorize.ma-3
                    v-dialog(
                        v-if="!isAuthenticated"
                        v-model="dialog"
                        width="300px"
                    )
                        template(#activator="{ on, attrs }")
                            v-btn(
                                v-on="on"
                                v-bind="attrs"
                            ) Авторизация
                        LogRegCard
                    v-menu(
                        v-if="isAuthenticated"
                        v-model="isAuthenticated"
                        transition="scroll-y-transition"
                        :close-on-content-click="false"
                        right
                    )
                        template(
                            #activator="{ on, attrs }"
                        )
                            v-btn(
                                v-bind="attrs"
                                v-on="on"
                            ) {{loggedUser.username}} 
                                v-icon(left) mdi-user
                        v-card(
                            mandatory
                        )
                            v-list
                                v-list-item(
                                    @click=""
                                )
                                    v-list-item-avatar
                                        img(src="avahero.jpg")
                                    v-list-content
                                        v-list-item-title Profile
                            v-card-actions(
                            )
                                v-btn(
                                    block
                                    outlined
                                    flat
                                    @click="logOut"
                                ) Logout
            template(
                v-slot:extension
            )
                nav.header-navigation
                    ul.v-tabs.d-flex
                        //- template(v-for="(link, i) in navlinks")
                        //-     v-hover v-slot:default="{ hover }"
                        //-         li.v-tab(
                        //-             :class="{ 'on-hover': hover }"
                        //-         ) 
                        //-             n-link(to='{link.href}') {{link.title}}

                        li.v-tab
                            n-link(to="/profile") Основное
                        li.v-tab
                            n-link(to="/photos") Фото
                        li.v-tab
                            n-link(to="/notes") Заметки
</template>

<script>
import LogRegCard from '~/components/LogRegCard'
import { mapGetters } from 'vuex'


export default {
    middleware: "auth",
    data: () => ({
        navlinks: [
            {
                title: "Основное",
                href: "/profile"
            },
            {
                title: "Фото",
                href: "/photos"
            },
            {
                title: "Заметки",
                href: "/notes"
            },
        ]
    }),
    computed: mapGetters(['isAuthenticated', 'loggedUser']),
    methods: {
        async logOut() {
            await this.$auth.logout()
        }
    },
    components: {
        LogRegCard
    }
}
</script>

<style>
    .v-application a {
        color: inherit;
        text-decoration: none;
    }
</style>