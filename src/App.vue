<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >

      <v-btn
        href="https://github.com/rafaGitHub1974/todo/"
        target="_blank"
        text
      >
        <span class="mr-2">TODO LIST (repository)</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>
