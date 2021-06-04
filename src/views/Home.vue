<template>
  <div id="container" class="svg-container" >
      <!-- <div>{{geojson.title}}</div> -->
  </div>
</template>

<script>
import * as d3 from 'd3'
import json from "../assets/sitedata.json"
export default {
   data(){
       return{
           mapJson:json,
           width:1000,
           height:1000,
        
            
       }
   },
   mounted(){
       this.getMapJson();
   },
   methods:{
       getMapJson(){
        console.log(this.mapJson.features)
        var canvas=d3.select("#container").append("svg")
                    .attr("width",this.width)
                    .attr("height",this.height)
                    
        var group=canvas.selectAll("g")
                        .data(this.mapJson.features)
                        .enter()
                        .append("g")
                        .attr("transform","translate(70,0)")
                        
        const projection=d3.geoMercator()
                        .center([116.4551,40.2539])
                        .scale(700);

        const path=d3.geoPath().projection(projection)

        group.append("path")
            .attr("d",path)
            .attr("fill","lightgrey")
            // .attr("stroke","#000")
            // .attr("stroke-width",1);
  
        // group.append("text")
        //     .attr("x",function(d){return path.centroid(d)[0] })
        //     .attr("y",function(d){return path.centroid(d)[1] })
        //     .attr("text-anchor","middle")
        //     .attr("font-size",8)
        //     .attr("color","#000")
        //     .text(function(d){return d.properties.name})

        
       }
   }
   
}
</script>

<style >
/* g :hover{
    fill: #fee;
} */
</style>