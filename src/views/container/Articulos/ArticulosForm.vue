<template>
  <v-container
    id="articulo-profile"
    fluid
    tag="section"
  >
    <v-row justify="center">
      <base-material-card icon="mdi-account-outline">
        <template v-slot:heading>
          <v-tabs
            v-model="tabs"
            background-color="transparent"
            slider-color="white"
          >
            <v-tab class="mr-3">
              <v-icon class="mr-2">
                mdi-account-key
              </v-icon>
              <!--{{ getTitleButton }}-->
              <h1>Articulos</h1>&nbsp;
            </v-tab>
          </v-tabs>
        </template>

        <v-card-text style="height: 100px; position: relative">
          <v-fab-transition>
            <v-btn
              fab
              dark
              small
              color="secondary"
              absolute
              right
              top
              @click="$router.go(-1)"
            >
              <v-icon>mdi-arrow-left</v-icon>
            </v-btn>
          </v-fab-transition>
        </v-card-text>

        <v-data-table
          :headers="headers"
          :items="desserts"
          class="elevation-1"
        >
          <!-- change table header color(or other properties) -->
          <template
            slot="headerCell"
            slot-scope="{ header }"
          >
            <span
              class="subheading font-weight-light text-general text--darken-3"
              v-text="header.text"
            />
          </template>
          <!-- inline editing data table -->
          <template v-slot:items="props">
            <td>
              <v-edit-dialog
                :return-value.sync="props.item.name"
                lazy
                @save="saveInline"
                @cancel="cancelInline"
                @open="openInline"
                @close="closeInline"
              >
                {{ props.item.name }}
                <template v-slot:input>
                  <v-text-field
                    v-model="props.item.name"
                    :rules="[max25chars]"
                    label="Edit"
                    single-line
                    counter
                  />
                </template>
              </v-edit-dialog>
            </td>
            <td class="t">
              {{ props.item.cant }}
            </td>
            <td class="text-right">
              <v-edit-dialog
                :return-value.sync="props.item.peso"
                large
                lazy
                persistent
                @save="saveInline"
                @cancel="cancelInline"
                @open="openInline"
                @close="closeInline"
              >
                <div>{{ props.item.peso }}</div>
                <template v-slot:input>
                  <div class="mt-3 title">
                    Update Peso
                  </div>
                </template>
                <template v-slot:input>
                  <v-text-field
                    v-model="props.item.peso"
                    :rules="[max25chars]"
                    label="Edit"
                    single-line
                    counter
                    autofocus
                  />
                </template>
              </v-edit-dialog>
            </td>
            <td class="">
              {{ props.item.precio }}
            </td>
            <td class="text-right">
              <v-edit-dialog
                :return-value.sync="props.item.desct"
                large
                lazy
                persistent
                @save="saveInline"
                @cancel="cancelInline"
                @open="openInline"
                @close="closeInline"
              >
                <div>{{ props.item.desct }}</div>
                <template v-slot:input>
                  <div class="mt-3 title">
                    Update Descuento
                  </div>
                </template>
                <template v-slot:input>
                  <v-text-field
                    v-model="props.item.desct"
                    :rules="[max25chars]"
                    label="Edit"
                    single-line
                    counter
                    autofocus
                  />
                </template>
              </v-edit-dialog>
            </td>
          </template>
        </v-data-table>
        <v-snackbar
          v-model="snack"
          :timeout="3000"
          :color="snackColor"
        >
          {{ snackText }}
          <v-btn
            flat
            @click="snack = false"
          >
            Close
          </v-btn>
        </v-snackbar>
      </base-material-card>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    data () {
      return {
        snack: false,
        snackColor: '',
        snackText: '',
        max25chars: v => v.length <= 25 || 'Input too long!',
        pagination: {},
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
          { text: 'Peso (g)',
            value: 'peso',
          },
          { text: 'Precio (Bs)',
            value: 'precio',
          },
          { text: 'Descuento (%)',
            value: 'desct',
          },
          { text: 'Iron (%)',
            value: 'iron',
          },
        ],
        desserts: [
          {
            name: 'Jabon',
            cant: 10,
            peso: 5,
            precio: 24000,
            desct: 0.1,
            iron: '1%',
          },
          {
            name: 'Crema Dental',
            cant: 5,
            peso: 1,
            precio: 1000,
            desct: 0.05,
            iron: '1%',
          },
          {
            name: 'Mayonesa',
            cant: 2,
            peso: 1,
            precio: 6000,
            desct: 0.1,
            iron: '7%',
          },
          {
            name: 'Pasta',
            cant: 25,
            peso: 40,
            precio: 55000,
            desct: 0.15,
            iron: '8%',
          },
          {
            name: 'Arroz',
            cant: 3,
            peso: 8,
            precio: 10500,
            desct: 0.2,
            iron: '16%',
          },
          {
            name: 'Maiz',
            cant: 2,
            peso: 4,
            precio: 2400,
            desct: 0.05,
            iron: '0%',
          },
          {
            name: 'Caraotas',
            cant: 10,
            peso: 5,
            precio: 240000,
            desct: 0.1,
            iron: '2%',
          },
          {
            name: 'Cera',
            cant: 1,
            peso: 1,
            precio: 240,
            desct: 0.1,
            iron: '45%',
          },
        ],
      }
    },
    methods: {
      save () {
        this.snack = true
        this.snackColor = 'success'
        this.snackText = 'Data saved'
      },
      cancel () {
        this.snack = true
        this.snackColor = 'error'
        this.snackText = 'Canceled'
      },
      open () {
        this.snack = true
        this.snackColor = 'info'
        this.snackText = 'Dialog opened'
      },
      close () {
        console.log('Dialog closed')
      },
    },
  }
</script>
