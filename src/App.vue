<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div>Music AI Improviser</div>
      <v-spacer></v-spacer>
      <div></div>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "App",

  data: () => ({
    //
  }),
});
</script>
