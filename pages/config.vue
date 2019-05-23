<template>
  <v-app config>
    <v-layout justify-center>
      <v-flex xs10 sm10 md10>
        <v-card>
          <v-card-title primary-title>
            <div class = "headline">
              Panel de Configuraciones
            </div>
          </v-card-title>
          <v-container>
            <v-layout row wrap>
              <v-flex xs12>
                <v-expansion-panel v-model="activePanel">

                  <v-expansion-panel-content>
                    <template v-slot:header>
                      <div class="subheading">Estilistas</div>
                    </template>
                    <v-card>
                      <v-container grid-list-xs>
                        <v-layout row wrap>
                          <v-flex xs10 sm10 md10 lg10>
                            <v-list subheader>
                              <v-list-tile
                                v-for="(stylist, i) in stylists"
                                :key="i"
                                avatar>
                                <v-list-tile-avatar>
                                  <v-icon light>person_pin</v-icon>
                                </v-list-tile-avatar>
                                <v-list-tile-content>
                                   <v-list-tile-title v-html="stylist.name"></v-list-tile-title>
                                </v-list-tile-content>
                                <!-- <v-list-tile-action>
                                  <v-icon @click="" color="blue">create</v-icon>
                                </v-list-tile-action> -->
                                <v-list-tile-action>
                                  <v-icon @click="confirmDeleteStylist(i)" color="red">delete</v-icon>
                                </v-list-tile-action>
                              </v-list-tile>
                              <v-list-tile avatar @click="addStylistDialog = true">
                                <v-list-tile-avatar>
                                  <v-icon light color="blue">add</v-icon>
                                </v-list-tile-avatar>
                                <v-list-tile-title>Agregar Estilista</v-list-tile-title>
                              </v-list-tile>
                              <v-dialog
                                v-model="addStylistDialog"
                                max-width="400"
                                transition="dialog-transition"
                              >
                                <v-card>
                                  <v-card-text>
                                    <v-text-field
                                      v-model="newStylistInput"
                                      label="Nuevo Estilista"
                                    ></v-text-field>
                                  </v-card-text>
                                  <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn color="blue darken-1" flat @click="addStylistDialog = false">Cancelar</v-btn>
                                    <v-btn color="blue darken-1" flat @click="confirmAddStylist">Agregar</v-btn>
                                  </v-card-actions>
                                </v-card>
                              </v-dialog>
                            </v-list>
                          </v-flex>
                        </v-layout>
                      </v-container>
                      <v-snackbar
                        v-model="sbStylistBool"
                        :color="sbStylistColor"
                        :timeout="5000"
                      >
                        {{sbStylistText}}
                        <v-btn
                          dark
                          flat
                          @click="sbStylistBool = false"
                        >
                          Cerrar
                        </v-btn>
                      </v-snackbar>
                    </v-card>
                  </v-expansion-panel-content>
                </v-expansion-panel>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card>
      </v-flex>
    </v-layout>
  </v-app>
</template>
<script>

import axios from 'axios'
export default {
  data() {
    return {
      activePanel: 0,
      sbStylistBool: false,
      sbStylistText: '',
      sbStylistColor: '',
      stylists: [],
      addStylistDialog: false,
      newStylistInput: ''
    }
  },
  methods: {
    async getStylists() {
      const res = await axios.get(`https://8nvvq3cryg.execute-api.us-east-1.amazonaws.com/Stage1/stylist`)
      console.log(res.data)
      this.stylists = res.data.Items
    },

    async getAppointments() {
      const res = await axios.get(`https://8nvvq3cryg.execute-api.us-east-1.amazonaws.com/Stage1/appointment`)
      console.log(res.data)
      this.stylists = res.data.Items
    },

    async addStylist(stylist) {
      try {
        this.addStylistDialog = false
        // const res = await axios.post(`http://${host}:8200/api/addEmail`, {
        //   email: email
        // })
        // if (res.data.message == "OK") {
          this.sbStylistColor = 'blue';
          this.sbStylistText = 'El estilista se ha agregado exitosamente.';
          this.newStylistInput = '';
          this.stylists.push({
            id: stylist,
            name: stylist
          });
        // } else {
        //   this.sbStylistColor = 'error'
        //   this.sbStylistText = res.data.message;
        // }
        this.sbStylistBool = true
        console.log(res.data)
      } catch(err) {
        console.error(err)
      }
    },
    async deleteStylist(index) {
      try {
        this.addStylistDialog = false
        // const res = await axios.post(`http://${host}:8200/api/addEmail`, {
        //   email: email
        // })
        // if (res.data.message == "OK") {
          this.sbStylistColor = 'blue';
          this.sbStylistText = 'El estilista se ha borrado exitosamente.';
          this.newStylistInput = '';
          this.stylists.splice(index, 1)
        // } else {
        //   this.sbStylistColor = 'error'
        //   this.sbStylistText = res.data.message;
        // }
        this.sbStylistBool = true
        console.log(res.data)
      } catch(err) {
        console.error(err)
      }
    },
    confirmAddStylist() {
      let stylist = this.newStylistInput;
      confirm(`¿Desea agregar al estilista ${stylist}`)
      && this.addStylist(stylist)
    },
    confirmDeleteStylist(index) {
      let stylist = this.stylists[index];
      confirm(`¿Desea eliminar al estilista ${stylist.name}`)
      && this.deleteStylist(index)
    }
  },
  created() {
    this.getStylists()
  }
}
</script>
