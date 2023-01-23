<template>
 
  <v-row class="fill-height">
   

      <button  style="margin: 5%; margin-right: 5%;" @click="displayMessage() + beforeCreate() + created() + beforeMount() + mounted()">Clique ici</button>
      <p  style="margin: 5%;" v-text="message"></p>  
      <button  style="margin-right: 2%;" @click="beforeUpdate() + changeMessage() + updated()">Change message</button>
      <button @click="destroyComponent() + beforeDestroy()">Destroy Component</button>
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
        <!--Création d'une liaison bidirectionnelle sur un élément d'entrée de formulaire avec le v-model .-->
        <v-text-field placeholder="Title"  v-model="title" :items="title"></v-text-field>
        <v-text-field placeholder="Start"  v-model="start" :items="start"></v-text-field>
        <v-text-field placeholder="End"    v-model="end" :items="end"></v-text-field>
        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          
          <v-btn
          
            color="primary"
            text
            @click="dialog = false; addEvent()"
            
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
          :events="events"
          @click:event="showEvent"
          @click:more="viewDay"
          @click:date="viewDay"
          @change="addEvent"
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
      message: '',
      events: [],
      title:"",
      start:"",
      end:"",
      value:'',
      colors: ['blue', 'indigo', 'deep-purple', 'cyan', 'green', 'orange', 'grey darken-1'],
      dialog: false,
    }),
    mounted() {
      this.$refs.calendar.checkChange()

    },
    methods: {
      displayMessage() {
        this.message = "Hello World!"
      },
      beforeCreate() {
        alert('Avant la création du composant')
      },
      created() {
        alert('Le composant a été créé')
      },
      beforeMount() {
        alert('Avant le montage du composant')
      },
      mounted(){
        alert('Le composant a été monté')
      },
      beforeUpdate() {
        alert('Avant la mise à jour des propriétés du composant')
      },
      updated() {
        alert('Les propriétés du composant ont été mises à jour')
      },
      beforeDestroy() {
        console.log('Avant la destruction du composant')
      },
      destroyed() {
        console.log('Le composant a été détruit')
      },
      addEvent(){
        //test pour savoir si ma fonctionne communique avec mon template
        console.log(this.title, this.start, this.end)
        //push de mes variable dans l'event qui est une list qui est vide
        //et un random pour mettre aleatoirement une couleur lors de la saisie d'un event
        this.events.push({
          name: this.title,
          start: new Date(this.start),
          end: new Date (this.end),
          color: this.colors[this.rnd(0, this.colors.length - 1)],
          })
        },
      rnd (a, b) {
        return Math.floor((b - a + 1) * Math.random()) + a
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
      changeMessage() {
        this.message = 'Message changed'
      },
      beforeDestroy() {
        this.$destroy()
        alert("Avant la destruction du composant")
      },
      destroyComponent() {
        this.$destroy()
        alert("le composant a été détruit")
      }
    },
  }
</script>






