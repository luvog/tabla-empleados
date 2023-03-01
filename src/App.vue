<template>
  <v-app>
    <v-app-bar
      app
      color="grey darken-3"
      class="text-h3"
      dark
    >
      <p class="mx-auto mt-3 light-blue--text darken-2">PORTAL DE EMPLEADOS</p>

      <v-spacer></v-spacer>
      
      <img alt="Overtek" src="./assets/logo.png" width="200px">
    </v-app-bar>

    <v-main>
      <TablaEmpleados/>
    </v-main>
  </v-app>
</template>

<script>
import TablaEmpleados from './components/TablaEmpleados';

export default {
  name: 'App',

  components: {
    TablaEmpleados,
  },

  data: () => ({
    //
  }),
};
</script>
