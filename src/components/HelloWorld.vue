<template>
  <v-row class="fill-height">
    <v-col>
      <v-img
          :src="require('../assets/transparent.png')"
          class="my-3"
          contain
          height="600"
        />
      <v-sheet height="64">
        <v-toolbar
          flat
        >
        <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="red lighten-2"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Create
        </v-btn>
      </template>

      <v-card>
        <v-card-title class="text-h5 grey lighten-2" >
          Create event
        </v-card-title>
        <v-text-field placeholder="Title"></v-text-field>
        <v-text-field placeholder="Start"></v-text-field>
        <v-text-field placeholder="End"></v-text-field>
        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="addEvent('Event Title', 'red')"
          >
            Agree
          </v-btn>
          <v-btn
            color="secondary"
            text
            @click="dialog = false"
              >
                Cancel
              </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
          <v-btn
            outlined
            class="mr-4"
            color="grey darken-2"
            @click="setToday"
          >
            Today
          </v-btn>
          <v-btn
            fab
            text
            small
            color="grey darken-2"
            @click="prev"
          >
            <v-icon small>
              mdi-chevron-left
            </v-icon>
          </v-btn>
          <v-btn
            fab
            text
            small
            color="grey darken-2"
            @click="next"
          >
            <v-icon small>
              mdi-chevron-right
            </v-icon>
          </v-btn>
          <v-toolbar-title v-if="$refs.calendar">
            {{ $refs.calendar.title }}
          </v-toolbar-title>
          <v-spacer></v-spacer>
          <v-menu
            bottom
            right
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                outlined
                color="grey darken-2"
                v-bind="attrs"
                v-on="on"
              >
                <span>{{ typeToLabel[type] }}</span>
                <v-icon right>
                  mdi-menu-down
                </v-icon>
              </v-btn>
            </template>
            <v-list >
              <v-list-item @click="type = 'day'">
                <v-list-item-title >Day</v-list-item-title>
              </v-list-item>
              <v-list-item @click="type = 'week'">
                <v-list-item-title>Week</v-list-item-title>
              </v-list-item>
              <v-list-item @click="type = 'month'">
                <v-list-item-title>Month</v-list-item-title>
              </v-list-item>
              <v-list-item @click="type = '4day'">
                <v-list-item-title>4 days</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
        </v-toolbar>
      </v-sheet>
      <v-sheet height="600">
        <v-calendar
          ref="calendar"
          v-model="focus"
          color="primary"
          :event-color="getEventColor"
          :type="type"
          @click:event="showEvent"
          @click:more="viewDay"
          @click:date="viewDay"
        ></v-calendar>
        <v-menu
          :close-on-content-click="false"
          offset-x
        >
          <v-card
            color="grey lighten-4"
            min-width="350px"
            flat
          >
            <v-toolbar
              dark
            >
              <v-btn icon>
                <v-icon>mdi-pencil</v-icon>
              </v-btn>
              <v-spacer></v-spacer>
              <v-btn icon>
                <v-icon>mdi-heart</v-icon>
              </v-btn>
              <v-btn icon>
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </v-toolbar>
            <v-card-text>
            </v-card-text>
            <v-card-actions>
              <v-btn
                text
                color="secondary"
                @click="selectedOpen = false"
              >
                Cancel
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-menu>
      </v-sheet>
    </v-col>
  </v-row>
</template>

<script>

  export default {
    data: () => ({
      focus: '',
      type: 'month',
      typeToLabel: {
        month: 'Month',
        week: 'Week',
        day: 'Day',
        '4day': '4 Days',
      },
      events: {
        Title:"Reda",
        Start:"2023-O1-09",
        End:"2023-O1-10",
        color:"red"
        },
      dialog: false,
    }),
    mounted() {
      this.$refs.calendar.checkChange()
    },
    methods: {
      // callEvent(){
      //   this.events.push(this.events)
      //   console.log(events)
      // },
      addEvent(eventTitle, eventColor) {
      this.$refs.calendar.addEvent({
        title: eventTitle,
        color: eventColor
      });
    },
    viewDay ({ date }) {
        this.focus = date
        var timestamp = Date.now()
        this.type = 'day'
        console.log(timestamp)
        console.log(date)
      },
      getEventColor(event) {
        return event.color
      },
      setToday () {
        this.focus = ''
      },
      prev () {
        this.$refs.calendar.prev()
      },
      next () {
        this.$refs.calendar.next()
      },
      showEvent ({ nativeEvent, event }) {
        const open = () => {
          requestAnimationFrame(() => requestAnimationFrame(() => this.selectedOpen = true))
        }
        if (this.selectedOpen) {
          this.selectedOpen = false
          requestAnimationFrame(() => requestAnimationFrame(() => open()))
        } else {
          open()
        }
        nativeEvent.stopPropagation()
      },
    },
  }
</script>