<template>  
    <v-row class="text-center">
      <v-col cols="12">
        <v-card elevation="2" class="mt-5">

          <v-card-title class="orange lighten-1">Tabla de empleados</v-card-title>

          <v-card-text>
            <v-data-table
              dense
              :headers="headers"
              :items="items"
              item-key="id"
              class="elevation-1"
            >
              <template v-slot:[`top`]>
                <v-row>
                  <v-col cols="4">
                      <v-text-field class="ml-4" v-model="nombre" type="text" label="Nombre del empleado"></v-text-field>
                  </v-col>
                  <v-col cols="4">
                      <v-switch v-model="salario" label="Salario menor de 200.000€" color="light-blue darken-3" hide-details></v-switch>
                  </v-col>
                  <v-col cols="4">
                      <v-switch v-model="edad" label="Edad mayor de 45 años" color="light-blue darken-3" hide-details></v-switch>
                  </v-col>
                </v-row>
              </template>              

              <template v-slot:[`footer.prepend`]>
                <v-btn @click="limpiar" depressed color="light-blue darken-2" class="white--text">Limpiar Filtros</v-btn>
              </template>
            </v-data-table>
          </v-card-text>
        </v-card>
      </v-col>      

      <v-dialog
        v-model="alerta"
        transition="dialog-bottom-transition"
        max-width="600"
        >
        <template v-slot:default="dialog">
          <v-card>
            <v-card-title class="orange">
              <span class="mdi mdi-alert mr-2 text-h4"></span>
              Error inesperado
            </v-card-title>

            <v-card-text>
              <div class="text-h5 pa-12">Ha ocurrido un error inesperado llamando a la API, inténtelo de nuevo más tarde.</div>
            </v-card-text>

            <v-card-actions class="justify-end">
              <v-btn
                text
                @click="dialog.value = false"
              >Cerrar</v-btn>
            </v-card-actions>
          </v-card>
        </template>
      </v-dialog>
    </v-row>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'TablaEmpleados',

    data: () => ({
      headers: [
        { text: 'Nombre', value: 'employee_name' },
        { text: 'Salario', value: 'employee_salary' },
        { text: 'Edad', value: 'employee_age' },
      ],
      nombre: "",
      salario: null,
      edad: null,
      alerta: false,
      empleados: [],
      items: [],
    }),

    watch: {
      nombre() {
        this.items = this.items.filter(empleado => empleado.employee_name.toUpperCase().includes(this.nombre.toUpperCase()))
      },

      salario() {
        if (this.salario !== null && this.salario) {
          this.items = this.items.filter(empleado => empleado.employee_salary < 200000)
        } else if (this.salario !== null && !this.salario) {
          this.items = this.items.filter(empleado => empleado.employee_salary >= 200000)
        }
      },

      edad() {
        if (this.edad !== null && this.edad) {
          this.items = this.items.filter(empleado => empleado.employee_age > 45)
        } else if (this.edad !== null && !this.edad) {
          this.items = this.items.filter(empleado => empleado.employee_age <= 45)
        }
      },
    },

    methods: {
      limpiar() {        
        this.nombre = ""
        this.salario = null
        this.edad = null
        this.items = this.empleados
      },
    },

    async created() {
      try {
        let response = await axios.get('https://dummy.restapiexample.com/api/v1/employees')
        this.empleados = response.data.data
        this.items = response.data.data
      } catch (error) {
        this.alerta = true
      }
    },
  }
</script>

<style>
  tbody tr:nth-of-type(odd) {
    background-color: #E1F5FE;
  }
</style>