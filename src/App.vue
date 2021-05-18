<template>
  <v-app>
    <header>
      <h2 class="text-center green white--text pa-1">
        CRUD Vuejs | Actividad I
      </h2>
    </header>
    <v-container>
      <v-row class="flex align-center justify-space-between my-5">
        <v-col class="justify-end" cols="12 text-center" sm="4">
          <h3>
            Total Construcciones:
            <span class="count">{{ construcciones.length }}</span>
          </h3>
        </v-col>
        <v-col class="justify-start" cols="12 text-center" sm="4">
          <v-btn
            @click="abrirModalCrear"
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
      <div v-if="construcciones.length > 0">
        <template>
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr class="green text-uppercase">
                  <th class="white--text text-left">ID</th>
                  <th class="white--text text-left">Tipo</th>
                  <th class="white--text text-left">Ancho</th>
                  <th class="white--text text-left">Alto</th>
                  <th class="white--text text-left">Materiales</th>
                  <th class="white--text text-left">Acciones</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="construccion in construcciones"
                  :key="construccion._id"
                >
                  <td>{{ construccion._id.substring(1, 9) }}...</td>
                  <td>{{ construccion.nombre }}</td>
                  <td>
                    {{ construccion.ancho }} <small>Metros de ancho</small>
                  </td>
                  <td>{{ construccion.alto }} <small>Metros de alto</small></td>
                  <td>
                    <span
                      v-for="(material, index) in construccion.materiales"
                      :key="`material-${index}`"
                      >{{ index > 0 ? ", " : "" }}{{ material }}</span
                    >
                  </td>
                  <td>
                    <v-btn
                      @click="eliminarConstruccion(construccion._id)"
                      class="mx-2"
                      fab
                      dark
                      small
                      color="error"
                    >
                      <v-icon dark> mdi-minus </v-icon>
                    </v-btn>
                    <v-btn
                      @click="abrirModalEditar(construccion)"
                      class="mx-2"
                      fab
                      dark
                      small
                      color="teal"
                    >
                      <v-icon dark> mdi-pencil </v-icon>
                    </v-btn>
                  </td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </template>
      </div>
      <div v-else class="container">
        <v-container>
          <p class="text-center">{{ datosVacios }}</p>
        </v-container>
      </div>
    </v-container>
    <v-row justify="center">
      <v-dialog v-model="modalConstruccion" persistent max-width="550px">
        <v-card>
          <v-card-title>
            <span class="headline">{{ tituloFormulario }}</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-form ref="validation">
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      :rules="validacionCampos"
                      v-model="construccion.nombre"
                      label="Construccion*"
                      required
                      hint="Indique el tipo de construcción"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      :rules="validacionCampos"
                      v-model="construccion.alto"
                      label="Indique el ancho*"
                      min="0"
                      type="number"
                      hint="el valor expresado sera en metros (m)"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field
                      :rules="validacionCampos"
                      v-model="construccion.ancho"
                      label="Indique el alto*"
                      min="0"
                      type="number"
                      hint="el valor expresado sera en metros (m)"
                      required
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" lg="12" xl="12" md="12">
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
                      placeholder="Selecciona o escribe"
                      no-data-text="Este material no esta disponible"
                      chips
                      :allow-overflow="false"
                    ></v-autocomplete>
                  </v-col>
                </v-row>
              </v-form>
            </v-container>
            <small
              >Todos los campos con un * son de caracter obligatorios</small
            >
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="cerrarModal">
              Cerrar
            </v-btn>
            <v-btn color="blue darken-1" text @click="enviarDatos">
              {{ textDinamicoBoton }}
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
    <footer class="footer">
      <small> {{ year }} &copy; Julio Rodriguez </small>
    </footer>
  </v-app>
</template>
 

<script>
import Swal from "sweetalert2";
import { url } from "@/constante.js";

export default {
  name: "App",
  data: () => ({
    modalConstruccion: false,
    datosVacios: "",
    esCrear: false,
    construccion: {
      nombre: "",
      ancho: "",
      alto: "",
      materiales: [],
    },
    construcciones: [],
    validacionCampos: [(valor) => !!valor || "Debes completar este campo."],
  }),
  mounted() {
    this.obtenerConstrucciones();
  },
  computed: {
    textDinamicoBoton() {
      return this.construccion._id ? "Editar" : "Crear";
    },
    tituloFormulario() {
      return this.construccion._id
        ? "Datos de la construcción en edición"
        : "Datos de la nueva construcción";
    },
    year() {
      return new Date().getFullYear();
    },
  },
  methods: {
    obtenerConstrucciones() {
      this.axios
        .get(`${url}/obtener`)
        .then(({ data }) => {
          if (data.length > 0) {
            this.construcciones = data;
          } else {
            this.construcciones = [];
            this.datosVacios = data.mensaje;
          }
        })
        .catch((e) => {
          this.mostrarAlerta(e.message, "error");
        });
    },
    mostrarAlerta(title, icon) {
      const alerta = Swal.mixin({
        toast: true,
        position: "top-end",
        showConfirmButton: false,
        timer: 2500,
        timerProgressBar: true,
      });

      return alerta.fire({
        icon,
        title,
      });
    },
    enviarDatos() {
      const esValido = this.comprobarDatos(this.construccion);
      if (!esValido) {
        return this.mostrarAlerta("Completa todos los campos", "error");
      }
      this.esCrear
        ? this.crearConstruccion(this.construccion)
        : this.editarConstruccion(this.construccion);
      this.modalConstruccion = false;
    },
    comprobarDatos({ nombre, ancho, alto, materiales }) {
      return !nombre || !ancho || !alto || !materiales.length ? false : true;
    },
    abrirModalCrear() {
      this.esCrear = true;
      this.modalConstruccion = true;
    },
    abrirModalEditar(datos) {
      this.construccion = { ...datos };
      this.esCrear = false;
      this.modalConstruccion = true;
    },
    cerrarModal() {
      this.modalConstruccion = false;
      this.establecerDatos();
    },
    crearConstruccion({ nombre, ancho, alto, materiales }) {
      this.axios
        .post(`${url}/agregar`, { nombre, ancho, alto, materiales })
        .then(({ data: { mensaje }, status }) => {
          if (status === 201) {
            this.mostrarAlerta(mensaje, "success");
            this.obtenerConstrucciones();
            this.establecerDatos();
          }
        })
        .catch((e) => {
          this.mostrarAlerta(e.message, "error");
          this.establecerDatos();
        });
    },
    editarConstruccion({ nombre, ancho, alto, materiales, _id }) {
      this.axios
        .put(`${url}/actualizar/${_id}`, { nombre, ancho, alto, materiales })
        .then((response) => {
          if (response.status === 200) {
            this.mostrarAlerta(response.data.mensaje, "success");
            this.obtenerConstrucciones();
            this.establecerDatos();
          }
        })
        .catch((e) => {
          this.mostrarAlerta(e.message, "error");
          this.establecerDatos();
        });
    },
    eliminarConstruccion(idConstruccion) {
      Swal.fire({
        title: "¿Quieres eliminar esta construcción?",
        text: "Una vez eliminada, no podras recuperas los datos",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#f14545",
        cancelButtonColor: "#4caf50",
        cancelButtonText: "Cancelar",
        confirmButtonText: "Eliminar",
      }).then((result) => {
        if (result.isConfirmed) {
          this.axios
            .delete(`${url}/eliminar/${idConstruccion}`)
            .then((response) => {
              if (response.status === 200) {
                this.mostrarAlerta(response.data.mensaje, "success");
                this.obtenerConstrucciones();
              }
            })
            .catch((e) => {
              this.mostrarAlerta(e.message, "errror");
            });
        }
      });
    },
    establecerDatos() {
      this.$refs.validation.resetValidation();
      this.construccion = {
        nombre: "",
        ancho: "",
        alto: "",
        materiales: [],
      };
      this.esCrear = false;
    },
  },
};
</script>
<style scoped>
#app {
  background-color: #eee;
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
.footer {
  font-weight: bold;
  color: #fff;
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  font-size: 1.5em;
  text-align: center;
  padding: 5px;
  background: #338045;
}
</style>