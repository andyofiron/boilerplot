

<template>
  <main>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter+Tight:wght@100;300&family=Montserrat:wght@100&display=swap" rel="stylesheet">

    <head class="title">
      <!-- title at top left -->
      <img width=30 src="./assets/logo.png">
      <h1 class="title">boilerplot</h1> 
      <div class="headerIconBox">
        <!-- this is all of the icons at the top of the page -->

        <button @click="isOpen = !isOpen" title="Info" class="headIcon"><img :height=iconHeight
            src="./assets/icons/information.png"></button>
        <button title="File" class="headIcon"><img :height=iconHeight src="./assets/icons/shape_move_back.png"></button>
        <button v-on:keypress.E ="addbox" @click="addBox" title="Create - E" class="headIcon"><img :height=iconHeight src="./assets/icons/add.png"></button>
        <button title="Focus/Unfocus - Alt" class="headIcon"><img :height=iconHeight
            src="./assets/icons/application.png"></button>
        <button  @click="editingMethod" title="Write - W" class="headIcon"><img :height=iconHeight src="./assets/icons/pencil.png"></button>
        <button title="Out Box - [" class="headIcon"><img :height=iconHeight src="./assets/icons/arrow_out.png"></button>
        <button title="In Box - ]" class="headIcon"><img :height=iconHeight src="./assets/icons/arrow_in.png"></button>

      </div>
    </head>

    

    <body  >
      <h2>root chart <span style="font-size: small; color: rebeccapurple; ">{{fancierEditModeDisplay()}}, selection: {{this.selectedBox}}</span></h2>
      <Infobox :open="isOpen"  @close="isOpen = !isOpen">
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
          <br />
          the current icons are the silk icons by famfamfam,<br />
          which is licensed under a Creative Commons Attribution 2.5 License.<br />
          the icons are available <a target="_blank" class="infoLink" href="http://www.famfamfam.com/lab/icons/silk/">here!</a><br />
        </p>
        <div class="infoCredits">
        <h3><img width=20 src="./assets/icons/vue.png"> powered by vue.js!</h3>
        <h3><img width=20 src="./assets/icons/information.png "> silk icons from famfamfam</h3>
        </div>
      
      </Infobox>
      
      

    </body>
    <Chart @chart_sbx="selectBox" :editingMode_chart="this.editingMode" :selectedID = "selectedBox" :isRoot = true :chartData="saveData.charts[currentChart]" >  </Chart>
    
    
  </main>
</template>

<script>
import { createApp, ref }  from 'vue'
import Infobox from './components/Infobox.vue'
import Box from './components/Box.vue'
import Chart from './components/Chart.vue'


export default {
  data() {
    return {
      
      saveData: {
        projectName: "The Fool's Journey",
        currentID:3,
        charts: {
          "_R_": [
            {holdsChart: true,location:[300,300],boxText:"Beginning",boxID:0}, // holds chart, location, text
            {holdsChart:false,location:[650,300],boxText:"Middle",boxID:1},
          ],
        0: [
            [false,[100,500],"The Fool starts their journey"]
        ]    
      }
    },
    iconHeight: 30,
    selectedBox: 0,
    currentID: 2,
    currentChart: "_R_",
    editingMode:false, //0 = selection, 1 = editing
    testBox: "<h1>big text??</h1>"
    }
  },
  components: { Infobox , Chart, Box },
  setup() {
    const isOpen = ref(false)
    return { isOpen }
    
    
    
  },
  
  methods: {
    genericWorking() {
      console.warn("IT WORKS!!!!!");
    },
    fancierEditModeDisplay(isEditing) {
      if (this.editingMode) {
        return("Editing Mode")
      }
      else
      {
        return("Drag Mode")
      }
    },
    toggleInfoBox() {
      this.isOpen = !this.isOpen
    },
    closeInfoBox() {
      this.isOpen = false
    },
    selectBox(id){
      console.log("recieved event")
      this.selectedBox = id
      console.log("selected box: " + this.id)
    },
    editingMethod(){
      this.editingMode = !this.editingMode
      console.log(this.selectedBox )
    },
    boxRange(card) {
      let br = []
      br.push([])
      br.push([])
      br[0].push(
        [card.location[0],card.location[0]+200]
        )
        br[1].push(
        [card.location[1],card.location[1]+150]
      )
      return { br }
    },
    addBox(){
      let boxLocation = [,];
      let badRanges = this.saveData.charts[this.currentChart].forEach(this.boxRange);
      //todo: finish this by adding the ranges a new box cant be in and adjusting accordingly
      let boxTemplate = {holdsChart: true,location:boxLocation,boxText:"...",boxID:this.currentID}
      this.saveData.charts[this.currentChart].push(boxTemplate);
      this.currentID++
      
    }
  
  }
}

</script>




<style scoped>

</style>
