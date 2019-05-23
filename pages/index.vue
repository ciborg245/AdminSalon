<template>
  <v-app id="inspire">
    <v-layout>
      <v-flex>

                            <v-dialog
                              v-model="usernameDialog"
                              max-width="400"
                              transition="dialog-transition"
                              persistent
                            >
                              <v-card>
                                <v-card-title primary-title>
                                  Usuario
                                </v-card-title>
                                <v-card-text>
                                  <v-text-field
                                    v-model="usernameInput"
                                    label="Nombre de Estilista"
                                    :error="validateStylist"
                                  ></v-text-field>
                                </v-card-text>
                                <v-card-actions>
                                  <v-spacer></v-spacer>
                                  <v-btn color="blue darken-1" flat @click="existStylist">Entrar</v-btn>
                                </v-card-actions>
                              </v-card>
                            </v-dialog>
        <v-sheet height="75%">
          <!-- now is normally calculated by itself, but to keep the calendar in this date range to view events -->
          <v-calendar
            ref="calendar"
            :now="today"
            :value="today"
            color="primary"
            type="week"
          >
            <!-- the events at the top (all-day) -->
            <template v-slot:dayHeader="{ date }">
              <template v-for="event in eventsMap[date]">
                <!-- all day events don't have time -->
                <div
                  v-if="!event.time"
                  :key="event.title"
                  class="my-event"
                  @click="open(event)"
                  v-html="event.title"
                ></div>
              </template>
            </template>
            <!-- the events at the bottom (timed) -->
            <template v-slot:dayBody="{ date, timeToY, minutesToPixels }">
              <template v-for="event in eventsMap[date]">
                <!-- timed events -->
                <div
                  v-if="event.time"
                  :key="event.title"
                  :style="{ top: timeToY(event.time) + 'px', height: minutesToPixels(event.duration) + 'px' }"
                  class="my-event with-time"
                  @click="open(event)"
                  v-html="event.title"
                ></div>
              </template>
            </template>
          </v-calendar>
        </v-sheet>
      </v-flex>
    </v-layout>
  </v-app>
</template>

<script>

import axios from 'axios'
export default {
  data: () => ({
    today: new Date().toISOString().substr(0, 10),
    evs: [
      {
        title: 'Checha Jacobs',
        date: '2019-07-01',
        time: '09:05',
        duration: 45
      },
      {
        title: 'Rafa de Leon',
        date: '2019-07-08',
        time: '09:05',
        duration: 45
      }
      // {
      //   title: 'Thomas\' Birthday',
      //   date: '2019-01-10'
      // },
    ],
    usernameDialog: true,
    usernameInput: '',
    stylists: [],
    stylistSelected: '',
    validateStylist: false,
    appointments: [],
    selectedAppointments: []
  }),

  computed: {
    // convert the list of events into a map of lists keyed by date
    eventsMap () {
      const map = {}
      this.evs.forEach(e => (map[e.date] = map[e.date] || []).push(e))
      return map
    }
  },
  mounted () {
    this.$refs.calendar.scrollToTime('08:00')
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
      this.appointments = res.data.Items
    },

    async getClientNameById(id) {
      let params = {
          id: id
        }

      const res = await axios.get(`https://8nvvq3cryg.execute-api.us-east-1.amazonaws.com/Stage1/client`, {
        params: params
      })

      return res.data.Items[0].name
    },

    open (event) {
      alert(event.title)
    },

    existStylist() {
      let search = this.usernameInput
      // this.stylistSelected = this.stylists[0]

      for (var stylist of this.stylists) {
        if (search == stylist.id) {
          this.stylistSelected = stylist
          this.usernameDialog = false
        }
      }

      this.validateStylist = true
      console.log(this.stylistSelected)
      this.searchAppointments()
    },

    async searchAppointments() {
      this.evs = []
      // console.log(this.stylistSelected.appointments)
      for (var app of this.appointments) {
        // console.log(app)
        if (this.stylistSelected.appointments.includes(app.id)) {
          this.evs.push({
            title: await this.getClientNameById(app.clientId),
            date: app.date.substr(0, 10),
            time: app.date.substr(11, 16),
            duration: 60
          })
        }
      }

      console.log(this.evs)
    }
  },


  created() {
    this.getAppointments()
    this.getStylists()
  }
}
</script>
<style>
  .my-event {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    border-radius: 2px;
    background-color: #1867c0;
    color: #ffffff;
    border: 1px solid #1867c0;
    font-size: 12px;
    padding: 3px;
    cursor: pointer;
    margin-bottom: 1px;
    left: 4px;
    margin-right: 8px;
    position: relative;

  }
  .with-time {
    position: absolute;
    right: 4px;
    margin-right: 0px;
  }
</style>
