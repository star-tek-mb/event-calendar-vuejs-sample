<template>
  <div class="event-calendar">
    <div class="calendar-buttons">
      <button @click="prev_month" type="button" class="btn btn-secondary">Prev</button>
      <button
        type="button"
        class="btn btn-secondary"
      >{{ view_date.toLocaleString(undefined, {year: 'numeric', month: 'long'}) }}</button>
      <button @click="next_month" type="button" class="btn btn-secondary">Next</button>
    </div>
    <div class="calendar-wrapper">
      <div class="calendar" v-bind:style="{ width: 5 + days_in_month(view_date) * 2 + 'rem'} ">
        <div class="calendar-fullrow">
          <span class="calendar-sticky">Events</span>
          <span class="calendar-cell" v-for="n in days_in_month(view_date)" :key="n">{{ n }}</span>
        </div>
        <div
          v-for="event_group in event_groups"
          :key="event_group.index"
          class="calendar-fullrow calendar-row"
        >
          <span class="calendar-sticky">{{ event_group }}</span>
          <span
            v-for="event in events.filter(e => e.group == event_group && (e.date_start.getMonth() <= view_date.getMonth() && (e.date_end ? e.date_end.getMonth() : Date.now().getMonth()) >= view_date.getMonth()))"
            :key="event.id"
            class="calendar-event"
            v-bind:style="{ background: event.color || 'lime', position: 'absolute', left: 5 + calc(event.date_start, event.date_end)[0] + 'rem', width: calc(event.date_start, event.date_end)[1] + 'rem', height: '1rem', display: 'inline-block' }"
            v-on:click="getInfo(event.id)"
          ></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    eventGroups: Array,
    eventObjects: Array,
  },
  data: function () {
    return {
      view_date: new Date(new Date().getFullYear(), new Date().getMonth(), 1),
      event_groups: this.eventGroups,
      events: this.eventObjects,
    };
  },
  methods: {
    next_month: function () {
      this.view_date = new Date(
        this.view_date.getFullYear(),
        this.view_date.getMonth() + 1,
        1
      );
    },
    prev_month: function () {
      this.view_date = new Date(
        this.view_date.getFullYear(),
        this.view_date.getMonth() - 1,
        1
      );
    },
    days_in_month: function (date) {
      return new Date(date.getFullYear(), date.getMonth() + 1, 0).getDate();
    },
    calc: function (start, end) {
      if (!end) {
        end = new Date();
      }

      var startem = (start.getDate() - 1) * 2;
      startem += start.getHours() / 12;
      if (this.view_date.getMonth() > start.getMonth()) {
        startem = 0;
      }

      var widthem = (end.getDate() - 1) * 2;
      widthem += end.getHours() / 12;
      widthem -= startem;
      if (this.view_date.getMonth() < end.getMonth()) {
        widthem = this.days_in_month(this.view_date) * 2 - startem;
      }

      return [startem, widthem];
    },
    getInfo: function(id) {
      console.log(this.events.find(e => e.id == id));
    },
  },
};
</script>

<style>
::-webkit-scrollbar {
  width: 5px;
  height: 5px;
}
::-webkit-scrollbar-track {
  box-shadow: inset 0 0 2px grey; 
}
::-webkit-scrollbar-thumb {
  background: red;
}
::-webkit-scrollbar-thumb:hover {
  background: #b30000; 
}

.event-calendar {
  width: 100%;
}
.calendar-wrapper {
  max-width: 640px;
  margin: 0 auto;
  overflow-x: scroll;
  box-sizing: border-box;
  border: 5px #b30000 solid;
}
.calendar {
  padding-right: 25px;
  padding-top: 25px;
  padding-bottom: 25px;
}
.calendar-fullrow {
  width: 100%;
  text-align: left;
  height: 1rem;
  margin-bottom: 1rem;
}
.calendar-buttons {
  width: 100%;
  text-align: center;
  margin-bottom: 2em;
}
.calendar-sticky {
  position: sticky;
  left: 0;
  background: white;
  width: 5rem;
  display: inline-block;
  z-index: 1;
  text-align: center;
}
.calendar-cell {
  width: 2rem;
  text-align: center;
  display: inline-block;
  box-sizing: border-box;
  border-right: 1px black solid;
  border-left: 1px black solid;
}
.calendar-row {
  position: relative;
}
.calendar-event:hover {
  opacity: 0.5;
  z-index: 1;
}
</style>
