<template>
  <div id="map">


  </div>
</template>

<script> 
import * as d3 from 'd3'
// import * as tip from 'd3-tip'

import ChinaMap from '../assets/sitedata.json'
import labelPoint from '../assets/city.json'

export default {
    data(){
        return{
            width:960,
            height:750,
           mapJson:ChinaMap,
           cityPoint:labelPoint
        }
    },
    mounted(){
         this.drawMap()
    },
    methods:{
        drawMap(){
            const box = d3.select('#map')
            const svg = box.append('svg')
                .attr('width',this.width)
                .attr('height',this.height)
            const g = svg.append('g')
                .attr('transform',"translate(200,100)")
            
            // var div = d3.select("body").append("div")   
            // .attr("class", "tooltip")               
            // .style("opacity", 0); 


            // let map_data = d3.json(ChinaMap)
            //使用墨卡托投影，并初始化相关参数c
            const projection = d3.geoMercator()
                .scale(800)
                .center([116.4551,40.2539])
                // .scale(1)
                // .fitSize([this.width,this.height],this.mapJson)

                // .translate([0,0])
                // .precision(0)
            //地理路径生成器
            const path = d3.geoPath()
                .projection(projection)


            const areas_layer = g.append('g') .attr('class','layer_areas')
            const areas_overlayer = g.append('g') .attr('class','overlayer_areas')
            const centers_layer = g.append('g') .attr('class','layer_center')
            // const image_layer = g.append('g') .attr('class','layer_image')
            const hover_layer = g.append('g').attr('class',"layer_hover")
            areas_layer
            .selectAll('path')
            .data(this.mapJson.features)
            .enter()
            .append('path')
        
            .attr('d',path)
            .attr('fill','#eee')
            .attr('stroke','#fff')
            .attr('stroke-dasharry','2 3')

            areas_overlayer
            .selectAll('path')
            .data(this.cityPoint.features)
            .enter()
            .append('path')
            .attr('class','areas_overlayer')
            .attr('d',path)
            .attr('fill','#ccc')
            .attr('stroke','#fff')

            // .on("mouseover",function(){
            //     image_layer
            //     .append('image')
            //     .attr('xlink:href','https://lorempixel.com/200/200/')
            //     .attr('height',200)
            //     .attr('width',250) 
            //     // .attr('fill','orange')
            // })
            // .on("mouseout",function(){
            //     // d3.select(this)
            //     image_layer
            //     .select('image')
            //     .remove()
            //     .attr("fill","#33E6FF")
            // })


            centers_layer
            .selectAll('.center_group')
            .data(this.cityPoint.features)  
            .enter()
            .append('g')
            .attr('class','center_group')
            .each(function(d){
                const el = d3.select(this)
                const [x,y] = projection(d.properties.center)
                const [x2,y2] =projection(d.properties.cp)

                
                //圆环
                const arc = d3.arc()

                var arcData={
                        innerRadius:11,
                        outerRadius:12,
                        startAngle:0,
                        endAngle:2*Math.PI
                    }
                var outData = arc(arcData)

            el 
                .append('path')
                .attr('transform',`translate(${x},${y})`)
                .attr('fill','#3396FF')
                .attr('fill-opacity',0.5)
                .attr('d',outData)
        
            
               var anCircle  = el 
                // append("circle") 圆
              .append("circle")
              .attr('class','excircle')
              .attr('cx',x)
              .attr('cy',y)
              .attr('r',6)
              .attr('stroke','none')
              .attr('fill','#3396FF')
              let run=()=>{
                  anCircle
              .transition()
              .duration(2000)
              .ease(d3.easeLinear)
              .delay(500)
              .attr("r",25)
              .attr('fill-opacity',0.1)
              .transition()
            //   .duration(1000)
              .ease(d3.easeLinear)
              .delay(500)
              .attr("r",6)
              .attr('fill-opacity',0)
              .on("end",run)
            }
            run()
            
           




            
            el 
                // append("circle") 圆
              .append("circle")
              .attr('class','excircle')
              .attr('cx',x)
              .attr('cy',y)
              .attr('r',6)
              .attr('stroke','none')
              .attr('fill','#3396FF')

              
            el
            .append('line')
            .attr('class','line-type')
            .attr('x1',x)
            .attr('y1',y)
            .attr('x2',x2)
            .attr('y2',y2)
            .attr('stroke','black')
            .attr('stroke-width','2px')

 
            el
              .append('text')
              .attr('x',x2-50)
              .attr('y',y2-10)
              .attr('dy','0.35em')
              .attr('fill','#3396FF')
              .text(d.properties.city)
              .on("mouseover",function(){
                  hover_layer
                    .append('text')
                    .attr('class','city')
                    .attr('x',x2-50)
                    .attr('y',y2-30)
                    .style("border","solid 1px")
                    // .style('opacity',1)
                    .text(d.properties.name)

              })
              .on("mouseout",function(){
                  hover_layer
                  .select('text')
                  .remove()
              })
            
            
              
              
              
            })

            // hover_layer
            // .selectAll(".center_group")
            // .data(this.mapJson.features)
            // .enter()
            // .append('text')
            // .text(function(d){
            //     return d.properties.name
            // })
            // .on("mouseout",function(){
            //     hover_layer
            //     .style("opacity",0)
            // })
            // .on("mouseover",function(){
            //     hover_layer
            //     .style("opacity",1)
            // })

        }
    },
}
</script>

<style>
g .center_group:hover{
    fill: red;
    cursor: pointer;
}
.layer_hover{
    background: blanchedalmond;
    fill: red;
}
.line-type{
    stroke-dasharray: 5,5;
}
/* .layer_center :hover{
    fill: red;
    cursor: pointer;
} */
</style>