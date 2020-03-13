<template>
  <v-app>
    <v-app-bar color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <h2>AraBoard</h2>
      </div>

      <v-spacer></v-spacer>

      <v-btn text>
        2020
      </v-btn>
      <v-btn text class="mx-2">
        2019
      </v-btn>
      <v-btn text>
        2018
      </v-btn>
    </v-app-bar>

    <v-content>
      <HelloWorld />
    </v-content>
  </v-app>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",

  components: {
    HelloWorld
  },

  data: () => ({
    //
  })
};
</script>
