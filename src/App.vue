

<template>
  <main>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter+Tight:wght@100;300&family=Montserrat:wght@100&display=swap"
      rel="stylesheet">

    <head class="title">
      <!-- title at top left -->
      <img width=30 src="./assets/logo.png">
      <h1 v-if="!debugEnabled" class="title">boilerplot</h1>
      <h1 v-else class="title">boilerdebug</h1>
      <div class="headerIconBox">
        <!-- this is all of the icons at the top of the page -->

        <button @click="isOpen = !isOpen" title="Info" class="headIcon"><img :height=iconHeight
            src="./assets/icons/information.png"></button>
        <button title="File" class="headIcon"><img :height=iconHeight src="./assets/icons/shape_move_back.png"></button>
        <button @keyup.e="addbox" @click="addBox" title="Create - Q" class="headIcon"><img :height=iconHeight
            src="./assets/icons/add.png"></button>
        <button @click="editingMethod" title="Mode Change - F1" class="headIcon"><img :height=iconHeight
            src="./assets/icons/application.png"></button>
        <button title="Out Box - [" class="headIcon"><img :height=iconHeight src="./assets/icons/arrow_out.png"></button>
        <button title="In Box - ]" class="headIcon"><img :height=iconHeight src="./assets/icons/arrow_in.png"></button>

      </div>
    </head>



    <body>
      <h2>root chart <span v-if="debugEnabled" style="font-size: small; color: rgb(158, 130, 184); ">{{ fancierEditModeDisplay() }}, selection:
          {{ this.selectedBox }}</span></h2>
      <div><img v-if="!editingMode" :height=iconHeight src="./assets/icons/application.png"><img v-else :height=iconHeight src="./assets/icons/pencil.png"></div>
      <Infobox :open="isOpen" @close="isOpen = !isOpen">
        <h2>
          about boilerplot - v0.0.1
        </h2>
        <p>
          boilerplot is a website created to help with the plotting of stories. <br />
          it may be used for many kinds of stories, e.g complex stories with a lot happening.<br />
          <br />
          boilerplot's system is a simple one, but it is very powerful. it revolves around<br />
          what i call "nested flowcharts." so what seperates this from normal flowcharts?<br />
          each flowcard, or what i call it, "box" can have another flowchart stored inside of it. <br />
          this allows for a lot of complexity to be stored in a small space. <br />
          think of an act structure. thats the main flowchart. each act can have its own flowchart<br />
          each of those acts can have their own flowchart and so on. <br />
          <br />
          it is a work in progress, and is currently in pre-alpha, or before functionality. <br />
        </p>
        <div class="infoCredits">
          <h3><img width=20 src="./assets/icons/vue.png"> powered by vue.js!</h3>
        </div>

      </Infobox>



    </body>
    <Chart @chart_sbx="selectBox" ref="thechart" :editingMode_chart="this.editingMode" :selectedID="selectedBox" :isRoot=true
      :chartData="saveData.charts[currentChart]"> </Chart>


  </main>
</template>

<script>
import { createApp, ref } from 'vue'
// import useHotkey, { HotKey } from 'vue3-hotkey'
import Infobox from './components/Infobox.vue'
import Box from './components/Box.vue'
import Chart from './components/Chart.vue'


export default {
  data() {
    return {

      saveData: {
        projectName: "The Fool's Journey",
        currentID: 3,
        charts: {
          "_R_": [
            { holdsChart: true, location: [300, 300], boxText: "Beginning", boxID: 0 }, // holds chart, location, text
            { holdsChart: false, location: [650, 300], boxText: "Middle", boxID: 1 },
          ],
          0: [
            [false, [100, 500], "The Fool starts their journey"]
          ]
        }
      },
      iconHeight: 30,
      selectedBox: 0,
      currentID: 2,
      currentChart: "_R_",
      editingMode: false, //0 = selection, 1 = editing
      debugEnabled: false,
      testBox: "<h1>big text??</h1>"
    }
  },
  components: { Infobox, Chart, Box },
  setup() {
    const isOpen = ref(false)
    return { isOpen }



  },
  created() {
    window.addEventListener('keydown', this.handleKeyDown);
  },

  methods: {
    genericWorking() {
      console.warn("IT WORKS!!!!!");
    },
    modeIcon(){
      if (this.editingMode === true) {
        return "./assets/icons/application.png"
      }
      else
      {
        return "./assets/icons/pencil.png"
      }
    },
    handleKeyDown(key) {
      if (key.keyCode === 112) {
        this.editingMethod();
      }
      else if (key.keyCode === 81 && !this.editingMode) {
        this.addBox();
      }
      else if (key.keyCode === 57 && !this.editingMode) {
        this.debugEnabled = !this.debugEnabled;
      }
      // arrow handler
      if (key.keyCode === 37) {
        this.arrowScroll(-1)
      }
      else if (key.keyCode === 38) {
        this.arrowScroll("Upper")
      }
      else if (key.keyCode === 39) {
        this.arrowScroll(1)
      }
      else if (key.keyCode === 40) {
        this.arrowScroll("Lower")
      }
    },
    arrowScroll(direction) {
      if (direction === 1 || direction === -1) {
        console.log("scrolling");
        this.selectedBox = (this.selectedBox + direction) % this.saveData.charts[this.currentChart].length;
        if (this.selectedBox < 0) {
          this.selectedBox = this.saveData.charts[this.currentChart].length - 1;
        }
      }
      else if (direction === "Lower" || direction === "Upper") {
        if (direction === "Lower") {
          this.selectedBox = 0;
        }
        else if (direction === "Upper") {
          this.selectedBox = this.saveData.charts[this.currentChart].length - 1;
        }
      }
      console.log(this.$refs.thechart.$refs.fc.$refs[this.selectedBox.toString()].$refs.draggableContainer.$refs.inputbox );

    },
    fancierEditModeDisplay(isEditing) {
      if (this.editingMode) {
        return ("Editing Mode")
      }
      else {
        return ("Drag Mode")
      }
    },
    toggleInfoBox() {
      this.isOpen = !this.isOpen
    },
    closeInfoBox() {
      this.isOpen = false
    },
    selectBox(id) {
      console.log("recieved event")
      this.selectedBox = id
      console.log("selected box: " + this.id)
    },
    editingMethod() {
      this.editingMode = !this.editingMode
      console.log(this.selectedBox)
    },
    boxRange(card) {
      let br = []
      br.push([])
      br.push([])
      br[0].push(
        [card.location[0], card.location[0] + 200]
      )
      br[1].push(
        [card.location[1], card.location[1] + 150]
      )
      return { br }
    },
    addBox() {
      let boxLocation = [,];
      let badRanges = this.saveData.charts[this.currentChart].forEach(this.boxRange);
      //todo: finish this by adding the ranges a new box cant be in and adjusting accordingly
      let boxTemplate = { holdsChart: true, location: boxLocation, boxText: "...", boxID: this.currentID }
      this.saveData.charts[this.currentChart].push(boxTemplate);
      this.currentID++

    }
  }
}

</script>




<style scoped></style>
