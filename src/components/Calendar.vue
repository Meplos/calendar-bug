<template>
  <v-container>
    <v-row justify="center">
      <v-col xl="6">
        <v-sheet height="600">
          <v-calendar
            ref="calendar"
            type="day"
            :events="events"
            :event-color="getEventColor"
            @contextmenu:event="showEvent"
            @contextmenu:time="log"
          >
          </v-calendar>
          <v-calendar
            ref="calendar"
            type="month"
            :events="events"
            :event-color="getEventColor"
            @contextmenu:event="showEvent"
            @contextmenu:day="log"
          >
          </v-calendar>
          <v-menu
            v-model="showMenu"
            :position-x="x"
            :position-y="y"
            absolute
            offset-y
            style="max-width: 600px"
          >
            <v-list>
              <v-list-item
                v-for="(item, index) in items"
                :key="index"
                @click="menuClick(item)"
              >
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import moment from "moment";
export default {
  data() {
    return {
      clickedEvent: null,
      showMenu: false,
      x: 0,
      y: 0,
      items: [
        { title: "Do Something" },
        { title: "Do Some More" },
        { title: "Do Another" },
        { title: "Do Whatever" },
      ],
    };
  },
  computed: {
    today() {
      return moment().format("YYYY-MM-DD");
    },
    events() {
      return [
        {
          name: "Red Event",
          start: `${this.today} 09:00`,
          end: `${this.today} 17:00`,
          color: "red",
        },
        {
          name: "Blue Event",
          start: `${this.today} 15:00`,
          end: `${this.today} 16:00`,
          color: "blue",
        },
      ];
    },
  },
  methods: {
    getEventColor(event) {
      return event.color;
    },
    menuClick(item) {
      console.log("action item:", item.title);
    },
    log(evt) {
      console.log(evt);
    },
    showEvent({ nativeEvent, event }) {
      console.log("event being passed", event);
      console.log("mouse event", nativeEvent);
      nativeEvent.preventDefault();
      this.showMenu = false;
      this.x = nativeEvent.clientX;
      this.y = nativeEvent.clientY;
      // make event useable by menuClick items
      this.clickedEvent = event;
      console.log("clickedEvent", this.clickedEvent);
      this.$nextTick(() => {
        this.showMenu = true;
      });
    },
  },
  mounted() {
    this.$refs.calendar.scrollToTime("06:00");
  },
};
</script>

<style></style>
