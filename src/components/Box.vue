<template>
    <div 
     class="chart-box" ref ="draggableContainer"  
     @mousedown="dragMouseDown " @mousemove="dragging" @mouseup="boxDrop" :style="isSelected() ? this.cStyle : this.sStyle">
        <input v-if="this.editingMode_box"  class="chart-boxContents"  v-model="this.boxText">
        <p v-else class="chart-boxContents">{{ this.boxText }}</p>


    </div>
</template>

<script>
//
import { WITH_MEMO } from '@vue/compiler-core';
import { isIntegerKey } from '@vue/shared';

//import Chart from './Chart.vue'
export default {
    props: { 
        // parent: {
        //     type: Chart,
        //     required: true
        // },
        holdsChart: {
            type: Boolean,
            required: true,
            default: false
        },
        
        screenLocation: {
            type: Array,
            required: true
        },
        savedboxText: {
            type: String,
            required: true
        },
        boxID:{
            type: Number,
            required: true
        },
        selectedID:{
            type: Number,
            required:true
        },
        editingMode_box:{
            type: Boolean,
            required:true
        },
        // contents: {
        //     type: Box,
        // }
    
        
        // add contents property with array of boxes
    },
    data: function () {
        return{
        pickedUp: false,
        pickupTime: 0,
        boxText:"",
        cStyle: {
            boxShadow: "0px 0px 10px " + "#8e6e91" //dropColor()
        },
        sStyle:
        {
            boxShadow: "0px 0px 15px " + "#ffffff" //dropColor()
        },
        }
    },
    mounted(){
        //console.log("box id emit: " + this.boxText )
        this.boxText=this.savedboxText;
        this.$refs.draggableContainer.style.left =  (this.screenLocation[0] -(200/2) )+ "px";
        this.$refs.draggableContainer.style.top = (this.screenLocation[1] - (150/2)) + "px";
    },
    methods: {
        dragMouseDown: function (Event) {
            this.$refs.draggableContainer.style.position = "absolute";
            this.pickedUp = !this.editingMode_box ;
            console.log("sending " + this.boxID)
            console.log("activity = " + this.editingMode_box)

            this.$emit('sbx', this.boxID );
            this.pickupTime = Date.now();
            

        },
        dragging: function (Event) {
            if (this.pickedUp == true && Date.now() - this.pickupTime > 20) {
                console.log("dragging");
                this.$refs.draggableContainer.style.left =  (Event.clientX - (200/2) )+ "px";
                this.$refs.draggableContainer.style.top = (Event.clientY - (150/2)) + "px";
            }
        },
        boxDrop: function () {
            this.pickedUp = false;
            console.log("mouse up");
        },
        dropColor(){
            dc = "#8e6e91"
            if (this.selectedID == this.boxID){
                dc="#fff0ff"
            }

            console.log(dc)
            return {dc}
        },
        isActive() {
            let activity = (this.selectedID == this.boxID && this.editingMode_box);
            return { activity }
        },
        isSelected() {
            let activity = (this.selectedID == this.boxID);
            return { activity }

        }
    }

};
</script>

<style>
.chart-header {
    background-color: blueviolet; 
    position: absolute;
    top: -15%;
    width: 300px;
    text-align: center;
    user-select: none;
    cursor: pointer;
}
.chart-boxContents {
    transition: 5ms;
    display:flex;
    user-select: none; 
    background-color: #8e6e91;
    color: aliceblue; 
    user-select: none;
    border-style:none;
    width: 90%;
    height: 90%;
    justify-content: center;
    text-align: center;
    align-items: center;
    font-family: 'Montserrat', sans-serif;
    font-size: 200%;
    overflow-wrap: break-word;
    overflow:visible;
    outline-offset: -8px;

}




.chart-box{
    width: 200px;
    height: 150px;
    position: absolute;
    display: flex;
    /* css that aligns the text to the vertical center */
    justify-content: center;
    align-items: center;
    text-align: center;
    background-color: #8e6e91;
    font-size: 11px;
    border-radius: 4%;
    z-index: 1;
    
}

#draggableContainer {
    position: absolute;
    background-color: red;
    
    top: 50px;
    left: 500px;
}


</style>
