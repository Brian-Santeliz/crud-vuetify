<template>
  <v-app>
    <header>
      <h2 class="text-center green white--text">CRUD Vuejs | Actividad I</h2>
    </header>
    <v-container>
      <v-row class="flex align-center justify-space-between my-5">
        <v-col class="justify-end" cols="12 text-center" sm="4">
          <h3>
            Total Construcciones:
            <span class="count">8</span>
          </h3>
        </v-col>
        <v-col class="justify-start" cols="12 text-center" sm="4">
          <v-btn color="green ligth white--text" elevation="2" fab>
            <v-icon dark x-large> mdi-plus-circle </v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>
<style scoped>
#app {
  background-color: #ebebeb;
}
.count {
  display: inline-block;
  padding: 0.25em 0.4em;
  font-size: 75%;
  font-weight: 700;
  font-size: 1rem;
  line-height: 1;
  text-align: center;
  white-space: nowrap;
  vertical-align: baseline;
  border-radius: 0.25rem;
  color: #fff;
  background-color: #28a745;
}
</style>