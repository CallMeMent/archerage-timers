<template>
  <div>
    <h1>Current Time: {{ currentTime }}</h1>
    <template
      v-for="(event, index) in getTodaysEvents.sort(
        (a, b) => this.determineTime(a) - this.determineTime(b)
      )"
    >
      <div
        v-if="determineTime(event) > MIN_DISPLAY_SECONDS"
        :key="event.name + 'div' + index"
      >
        {{ event.name }}
        <vue-countdown
          :seconds="determineTime(event)"
          :start="true"
        ></vue-countdown>
      </div>
    </template>
  </div>
</template>

<script>
import moment from "moment";
import { events } from "@/data/events.js";
import VueCountdown from "@dmaksimovic/vue-countdown";

export default {
  name: "Countdown",
  components: {
    "vue-countdown": VueCountdown,
  },
  data() {
    return {
      events: events,
      MIN_DISPLAY_SECONDS: -1000,
    };
  },
  methods: {
    determineTime(event) {
      // let newDate = moment().set({})
      let eventTime = moment.utc(event.time, "HH:mm a");
      let now = moment.utc();
      let diff = eventTime.diff(now, "seconds");
      return diff;
    },
    parseDayToString(day) {
      switch (day) {
        case 0:
          return "Sunday";
        case 1:
          return "Monday";
        case 2:
          return "Tuesday";
        case 3:
          return "Wednesday";
        case 4:
          return "Thursday";
        case 5:
          return "Friday";
        case 6:
          return "Saturday";
      }
    },
  },
  computed: {
    getTodaysEvents: () => {
      const today = moment().day();
      return events.filter((e) => e.days.includes(today));
    },
    currentTime: () => {
      return moment().toLocaleString();
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
