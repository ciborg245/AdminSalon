<template>
  <v-app id="inspire">
    <v-layout>
      <v-flex>
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
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  data: () => ({
    today: '2019-01-08',
    evs: [
      {
        title: 'Checha Jacobs',
        date: '2019-01-07',
        time: '09:05',
        duration: 45
      },
      {
        title: 'Rafa de Leon',
        date: '2019-01-08',
        time: '09:05',
        duration: 45
      },
      // {
      //   title: 'Thomas\' Birthday',
      //   date: '2019-01-10'
      // },
    ]
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
    open (event) {
      alert(event.title)
    }
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
