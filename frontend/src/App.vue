<template>
  <v-app>
    <v-app-bar
      app
      color="deep-orange"
      dark
    >
      <div class="d-flex align-center container font-weight-black">
        Thao Hien's To-do List App
      </div>
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
