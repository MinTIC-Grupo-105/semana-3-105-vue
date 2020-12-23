<template>
  <div id="app">
    <v-app id="inspire">
      <v-data-table
        :headers="headers"
        :items="categorias"
        sort-by="nombre"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Categorias</v-toolbar-title>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  color="primary"
                  dark
                  class="mb-2"
                  v-bind="attrs"
                  v-on="on"
                >
                  Agregar Categoria
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">{{ formTitle }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12">
                        <v-text-field
                          v-model="editedItem.nombre"
                          label="Nombre"
                        ></v-text-field>
                      </v-col>
                      <v-col cols="12">
                        <v-textarea
                          counter="250"
                          auto-grow
                          no-resize
                          v-model="editedItem.descripcion"
                          label="Descripcion"
                        ></v-textarea>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="close">
                    Cancel
                  </v-btn>
                  <v-btn color="blue darken-1" text @click="save"> Save </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <v-dialog v-model="dialogDelete" max-width="500px">
              <v-card>
                <v-card-title class="headline"
                  >Are you sure you want to delete this item?</v-card-title
                >
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" text @click="closeDelete"
                    >Cancel</v-btn
                  >
                  <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                    >OK</v-btn
                  >
                  <v-spacer></v-spacer>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-icon small class="mr-2" @click="editItem(item)">
            mdi-pencil
          </v-icon>
          <v-icon medium @click="deleteItem(item)">
            <template v-if="item.estado"> mdi-toggle-switch</template>
            <template v-else> mdi-toggle-switch-off-outline</template>

             
          </v-icon>
        </template>
        <template v-slot:no-data>
          <v-btn color="primary" @click="initialize"> Reset </v-btn>
        </template>
      </v-data-table>
    </v-app>
    <pre>
      {{ $data.categorias }}
    </pre>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      { text: "ID", value: "id" },
      {
        text: "Categoria",
        align: "start",
        sortable: true,
        value: "nombre",
      },
      { text: "Descripcion", value: "descripcion" },
      { text: "Estado", value: "estado" },
      { text: "Acciones", value: "actions", sortable: false },
    ],
    desserts: [],
    categorias: [],
    editedIndex: -1,
    editedItem: {
      nombre: "",
      descripcion: "",
      estado: 0,
    },
    defaultItem: {
      nombre: "",
      descripcion: "",
      estado: 0,
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },

  created() {
    this.initialize();
    this.list();
  },

  methods: {
    initialize() {
      this.desserts = [
        {
          nombre: "Frozen Yogurt",
          descripcion: 159,
          estado: 6.0,
        },
      ];
    },

    list() {
      axios
        .get("http://localhost:3000/api/categoria/list")
        .then((response) => {
          this.categorias = response.data;
          console.log(this.categorias);
        })
        .catch((error) => {
          console.log(error);
        });
    },

    editItem(item) {
      this.editedIndex = item.id;
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    deleteItem(item) {
      this.editedIndex = item.id;
      this.editedItem = Object.assign({}, item);
      this.dialogDelete = true;
    },

    deleteItemConfirm() {
      this.desserts.splice(this.editedIndex, 1);
      if (this.editedItem.estado == 1) {
        // put
        axios
          .put("http://localhost:3000/api/categoria/deactivate", {
            id: this.editedItem.id,
          })
          .then((response) => {
            this.list();
          })
          .catch((error) => {
            return error;
          });
      } else {
        // post
        axios
          .put("http://localhost:3000/api/categoria/activate", {
            id: this.editedItem.id,
          })
          .then((response) => {
            this.list();
          })
          .catch((error) => {
            return error;
          });
        this.desserts.push(this.editedItem);
      }
      this.closeDelete();
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        // put
        axios
          .put("http://localhost:3000/api/categoria/update", {
            id: this.editedItem.id,
            nombre: this.editedItem.nombre,
            descripcion: this.editedItem.descripcion,
          })
          .then((response) => {
            this.list();
          })
          .catch((error) => {
            return error;
          });
      } else {
        // post
        axios
          .post("http://localhost:3000/api/categoria/add", {
            id: this.editedItem.id,
            estado: 1,
            nombre: this.editedItem.nombre,
            descripcion: this.editedItem.descripcion,
          })
          .then((response) => {
            this.list();
          })
          .catch((error) => {
            return error;
          });
        this.desserts.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>