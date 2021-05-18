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
          <v-btn
            @click="dialog = true"
            color="green ligth white--text"
            elevation="2"
            fab
          >
            <v-icon dark x-large> mdi-plus-circle </v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <template>
        <v-simple-table>
          <template v-slot:default>
            <thead>
              <tr class="green text-uppercase">
                <th class="white--text text-left">ID</th>
                <th class="white--text text-left">Nombre</th>
                <th class="white--text text-left">Ancho</th>
                <th class="white--text text-left">Alto</th>
                <th class="white--text text-left">Materiales</th>
                <th class="white--text text-left">Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>1</td>
                <td>Ejemplo</td>
                <td>20</td>
                <td>30</td>
                <td>Nose, ejemplo, otro</td>
                <td>
                  <v-btn class="mx-2" fab dark small color="error">
                    <v-icon dark> mdi-minus </v-icon>
                  </v-btn>
                  <v-btn class="mx-2" fab dark small color="teal">
                    <v-icon dark> mdi-pencil </v-icon>
                  </v-btn>
                </td>
              </tr>
              <tr>
                <td>1</td>
                <td>Ejemplo</td>
                <td>20</td>
                <td>30</td>
                <td>Nose, ejemplo, otro</td>
                <td>
                  <v-btn class="mx-2" fab dark small color="error">
                    <v-icon dark> mdi-minus </v-icon>
                  </v-btn>
                  <v-btn class="mx-2" fab dark small color="teal">
                    <v-icon dark> mdi-pencil </v-icon>
                  </v-btn>
                </td>
              </tr>
              <tr>
                <td>1</td>
                <td>Ejemplo</td>
                <td>20</td>
                <td>30</td>
                <td>Nose, ejemplo, otro</td>
                <td>
                  <v-btn class="mx-2" fab dark small color="error">
                    <v-icon dark> mdi-minus </v-icon>
                  </v-btn>
                  <v-btn class="mx-2" fab dark small color="teal">
                    <v-icon dark> mdi-pencil </v-icon>
                  </v-btn>
                </td>
              </tr>
              <tr>
                <td>1</td>
                <td>Ejemplo</td>
                <td>20</td>
                <td>30</td>
                <td>Nose, ejemplo, otro</td>
                <td>
                  <v-btn class="mx-2" fab dark small color="error">
                    <v-icon dark> mdi-minus </v-icon>
                  </v-btn>
                  <v-btn class="mx-2" fab dark small color="teal">
                    <v-icon dark> mdi-pencil </v-icon>
                  </v-btn>
                </td>
              </tr>
              <tr>
                <td>1</td>
                <td>Ejemplo</td>
                <td>20</td>
                <td>30</td>
                <td>Nose, ejemplo, otro</td>
                <td>
                  <v-btn class="mx-2" fab dark small color="error">
                    <v-icon dark> mdi-minus </v-icon>
                  </v-btn>
                  <v-btn class="mx-2" fab dark small color="teal">
                    <v-icon dark> mdi-pencil </v-icon>
                  </v-btn>
                </td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </template></v-container
    >

    <v-row justify="center">
      <v-dialog v-model="dialog" persistent max-width="600px">
        <v-card>
          <v-card-title>
            <span class="headline">Datos de la construcción</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field
                    v-model="construccion.nombre"
                    label="Tipo de construcción*"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field
                    v-model="construccion.alto"
                    label="Indique el ancho*"
                    type="number"
                    hint="el valor expresado sera en metros (m)"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" md="4">
                  <v-text-field
                    v-model="construccion.ancho"
                    label="Indique el alto*"
                    type="number"
                    hint="el valor expresado sera en metros (m)"
                    required
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6">
                  <v-autocomplete
                    v-model="construccion.materiales"
                    :items="[
                      'Cemento',
                      'Barro y arcilla',
                      'Madera',
                      'Hormigón',
                      'Ladrillo',
                      'Vidrio',
                      'Acero',
                      'Aluminio',
                      'Zinc',
                    ]"
                    label="Materiales usados*"
                    multiple
                  ></v-autocomplete>
                </v-col>
              </v-row>
            </v-container>
            <small
              >Todos los campos con un * son de caracter obligatorios</small
            >
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="blue darken-1"
              text
              @click="
                dialog = false;
                construccion.materiales = [];
              "
            >
              Close
            </v-btn>
            <v-btn color="blue darken-1" text @click="dialog = false">
              Save
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
    {{ construccion.materiales }}
  </v-app>
</template>
 

<script>
export default {
  name: "App",
  data: () => ({
    dialog: false,
    construccion: {
      nombre: "",
      ancho: "",
      alto: "",
      materiales: [],
    },
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