<template>
  <v-container
    id="data-tables"
    tag="section"
  >
    <base-material-card
      color="indigo"
      icon="mdi-vuetify"
      inline
      class="px-5 py-3"
    >
      <template v-slot:after-heading>
        <div class="display-2 font-weight-light">
          {{ $t('articulos.title') }}
        </div>
      </template>

      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        class="ml-auto"
        label="Search"
        hide-details
        single-line
        style="max-width: 250px;"
      />

      <v-dialog
        v-model="dialog"
        max-width="800px"
      >
        <template v-slot:activator="{ on }">
          <v-btn
            color="primary"
            fab
            dark
            large
            fixed
            right
            bottom
            class="mb-2"
            v-on="on"
          >
            <v-icon>mdi-plus</v-icon>
          </v-btn>
        </template>

        <v-card>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-text-field
                    v-model="editedItem.name"
                    label="Nombre"
                  />
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-text-field
                    v-model="editedItem.cant"
                    label="Cantidad"
                    type="number"
                  />
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-text-field
                    v-model="editedItem.peso"
                    label="Peso"
                  />
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-text-field
                    v-model="editedItem.precio"
                    label="Precio (Bs)"
                  />
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-text-field
                    v-model="editedItem.desct"
                    label="Descuento (%)"
                  />
                </v-col>
              </v-row>
            </v-container>
          </v-card-text>

          <v-card-actions>
            <v-spacer />
            <v-btn
              color="red darken-1"
              flat
              @click="close"
            >
              <v-icon
                small
                v-text="'mdi-window-close'"
              />
            </v-btn>
            <v-btn
              color="blue darken-1"
              flat
              @click="save"
            >
              <v-icon
                small
                v-text="'mdi-content-save'"
              />
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-divider class="mt-3" />
      <v-data-table
        :headers="headers"
        :items="desserts"
        class="elevation-1"
      >
        <template
          slot="headerCell"
          slot-scope="{ header }"
        >
          <span
            class="subheading font-weight-light text-general text--darken-3"
            v-text="header.text"
          />
        </template>
        <template
          slot="headerCell"
          slot-scope="{ header }"
        >
          <span
            class="subheading font-weight-light text-general text--darken-3"
            v-text="header.text"
          />
        </template>

        <template v-slot:item.actions="props">
          <!--<v-btn

            :key="2"
            color="blue"
            fab
            class="px-1 ml-1"
            x-small
            @click="showItem(item)"
          >
            <v-icon
              small
              v-text="'mdi-eye'"
            />
          </v-btn>-->
          <v-btn

            :key="3"
            color="primary"
            fab
            class="px-1 ml-1"
            x-small
            @click="editItem(props.item)"
          >
            <v-icon
              small
              v-text="'mdi-pencil'"
            />
          </v-btn>
          <v-btn

            :key="4"
            color="secondary"
            fab
            class="px-1 ml-1"
            x-small
            @click="deleteItem(props.item)"
          >
            <v-icon
              small
              v-text="'mdi-delete'"
            />
          </v-btn>
        </template>
        <!--<template v-slot:no-data>
          <v-btn
            color="primary"
            @click="initialize"
          >
            Reset
          </v-btn>
        </template>-->
      </v-data-table>
    </base-material-card>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      headers: [
        {
          text: 'Nombre',
          align: 'left',
          sortable: false,
          value: 'name',
        },
        { text: 'Cantidad',
          value: 'cant',
        },
        { text: 'Peso',
          value: 'peso',
        },
        { text: 'Precio (Bs)',
          value: 'precio',
        },
        { text: 'Descuento (%)',
          value: 'desct',
        },
        { text: 'Actions',
          value: 'actions',
          sortable: false,
        },
      ],
      desserts: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        cant: 0,
        peso: 0,
        precio: 0,
        desct: 0,
      },
      defaultItem: {
        name: '',
        cant: 0,
        peso: 0,
        precio: 0,
        desct: 0,
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
        this.desserts = [
          {
            name: 'Jabon',
            cant: 10,
            peso: 5,
            precio: 24000,
            desct: 0.1,
          },
          {
            name: 'Crema Dental',
            cant: 5,
            peso: 1,
            precio: 1000,
            desct: 0.05,
          },
          {
            name: 'Mayonesa',
            cant: 2,
            peso: 1,
            precio: 6000,
            desct: 0.1,
          },
          {
            name: 'Pasta',
            cant: 25,
            peso: 40,
            precio: 55000,
            desct: 0.15,
          },
          {
            name: 'Arroz',
            cant: 3,
            peso: 8,
            precio: 10500,
            desct: 0.2,
          },
          {
            name: 'Maiz',
            cant: 2,
            peso: 4,
            precio: 2400,
            desct: 0.05,
          },
          {
            name: 'Caraotas',
            cant: 10,
            peso: 5,
            precio: 240000,
            desct: 0.1,
          },
          {
            name: 'Cera',
            cant: 1,
            peso: 1,
            precio: 240,
            desct: 0.1,
          },
        ]
      },

      editItem (item) {
        this.editedIndex = this.desserts.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.desserts.indexOf(item)
        confirm('Are you sure you want to delete this item?') && this.desserts.splice(index, 1)
      },

      showItem (item) {

      },

      close () {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },

      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.desserts[this.editedIndex], this.editedItem)
        } else {
          this.desserts.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>
