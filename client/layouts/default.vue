<template>
    <v-app>
        <v-app-bar app color="primary" dark>
            <v-toolbar-title>nuxt-vuetify-graphql-pinia-starter</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn to="/">Home</v-btn>
            <v-btn to="/launch">Launch</v-btn>
        </v-app-bar>
        <v-main>
            <slot></slot>
        </v-main>
        <v-footer app color="primary" dark>
            <span>&copy; 2023 nuxt-vuetify-graphql-pinia-starter</span>
        </v-footer>
    </v-app>
</template>
<script>
export default {
    name: 'DefaultLayout',
}
</script>
