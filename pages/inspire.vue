<template>
  <v-app config>
    <v-layout>
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
                      <div class="subheading">Usuarios</div>
                    </template>
                    <v-card>
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
export default {
  data() {
    return {
      activePanel: 0,
    }
  }
}
</script>
