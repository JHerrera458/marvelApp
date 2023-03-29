<template>
  <v-app dark>
    <v-main>
      <v-app-bar absolute>
        <v-app-bar-nav-icon></v-app-bar-nav-icon>
        <v-toolbar-title>Marvel App</v-toolbar-title>
      </v-app-bar>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
    }
  }
}
</script>
