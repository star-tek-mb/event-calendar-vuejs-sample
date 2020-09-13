<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <EventCalendar :event-groups="eventGroups" :event-objects="eventObjects" />
  </div>
</template>

<script>
import EventCalendar from "./components/EventCalendar.vue";

export default {
  name: "App",
  components: {
    EventCalendar,
  },
  data: function () {
    return {
      eventGroups: ['room1', 'room2', 'room3'],
      eventObjects: [
        {
          id: 1,
          group: "room1",
          color: "purple",
          date_start: new Date(2020, 8, 6, 12),
          date_end: new Date(2020, 8, 10, 0)
        },
        {
          id: 2,
          group: "room2",
          date_start: new Date(2020, 8, 6, 12),
          date_end: new Date(2020, 10, 1, 12)
        },
      ],
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
