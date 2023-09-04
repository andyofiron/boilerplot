<template> 
    <div class="flowChart" v-for="card in chartData"  >
        <Box @sbx = "selectBox"
        :editingMode_box="editingMode_chart" 
        :selectedID="selectedID" 
        :boxID="card.boxID" 
        :holdsChart="card.holdsChart" 
        :screenLocation="card.location" 
        :savedboxText="card.boxText">
        </Box>
        <!-- <Box @sbx = "selectBox(id)" :holdsChart=false :screenLocation=[] :boxText="item"></Box> -->
    </div>

</template>

<script>
import { createApp } from 'vue'
import Box from "./Box.vue";

export default {
    props: {
        component: {
            Box,
        },
        isRoot: {
            type: Boolean,
            required: true
        },
        chartData: {
            type: Array
        },
        selectedID:{
            type:Number,
            required:true,
        },
        editingMode_chart:{
            type: Boolean,
            required:true
        }
    },
    mounted() {
        this.chartChanged();
        console.log(this.chartData);
    },
    methods: {
        chartChanged() {
            console.log("setting chart up!");
            this.$emit("chartchange");
        },
        selectBox(id){
            this.$emit("chart_sbx", id);
            console.log("Emitted: " + id)
        }
    },
    components: { Box }
};
</script>