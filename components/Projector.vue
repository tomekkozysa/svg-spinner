<template>
<svg :width="width+'px'" :height="height+'px'" ref="svg" xmlns="http://www.w3.org/2000/svg" id="projector">
    <g>
           <SegmentLine :size="thickness" :x1="d.x1" :y1="d.y1" :x2="d.x2" :y2="d.y2" 
            v-for="d in stripes" :key="d.id" :colour="getColour(d.id)">

        <animate v-if="animate" :dur="time*1000+'ms'" repeatCount="indefinite" attributeName="stroke"
            :values="colorsequence" 
            calcMode="splines" 
            :begin="time*1000/count*d.id+'ms'" 
        />
     
        </SegmentLine>

    
           <!-- <Segment :r="r" :cx="d.cx" :cy="d.cy" 
        v-for="d in cells" :key="d.id" :fill="getColour(d.id)">

        <animate v-if="animate" :dur="time*1000+'ms'" repeatCount="indefinite" attributeName="fill"
            :values="colorsequence" 
            calcMode="splines" 
            :begin="time*1000/count*d.id+'ms'" 
        />
     
        </Segment>

     -->
            
    </g>
</svg>
</template>
<script>
import Segment from '~/components/Segment.vue'
import SegmentLine from '~/components/SegmentLine.vue'
export default {
    components:{
        SegmentLine,
        Segment,
    },
    data(){
        return{ 
         animate:true,
        //  colors:["#9400D3","#4B0082","#0000FF","#00FF00","#FFFF00","#FF7F00","#FF0000"],
        //  colors: ["#ff0000", "#ff7f00", "#ffff00", "#00ff00", "#0000ff", "#4b0082", "#9400d3"],
        // colors:["rgba(148, 0, 211,1)","rgba(75, 0, 130,1)","rgba(0, 0, 255,1)","rgba(0, 255, 0,1)","rgba(255, 255, 0,1)","rgba(255, 127,1)","rgba(255, 0 , 0,1)"],
        colors:["violet","indigo","blue","lightgreen","yellow","darkorange","red"],
        // colors:["black","white"]
        // colors:["black","AQUAMARINE"]
         

        }
    },
    props:{
        width:Number,
        height:Number,        
        count:null,
        radius:null,
        time: null,
        r:null,
        thickness:null,
        dotspeed:null,
    },
    mounted(){},
    computed:{
        center:function(){return {x:this.width/2,y:this.height/2}},
        anglestep:function(){ return 360/this.count},
        timestep:function(){ return this.time/(this.count+1);},
        colorsequence:function(){ return this.colors.join(";");
        },
       
        cells:function(){
            let d={};
            let data = [];
            for( let i=0;i<this.count;i++){
                let pc = this.polarToCartesian(this.center.x,this.center.y,this.radius,i*this.anglestep); 
                
                d = { 
                    id:i,
                    cx:parseFloat(pc.x).toFixed(2),
                    cy:parseFloat(pc.y).toFixed(2)
                    // cx:parseInt(pc.x),
                    // cy:parseInt(pc.y)
                }
                data.push(d);
             }
             return data;
        },
        stripes:function(){
            let d={};
            let data = [];
            for( let i=0;i<this.count;i++){
                let pc = this.polarToCartesian(this.center.x,this.center.y,this.radius,i*this.anglestep); 
                let pc_end = this.polarToCartesian(this.center.x,this.center.y,this.radius-this.r,i*this.anglestep); 
                
                d = { 
                    id:i,
                    x1:parseFloat(pc.x).toFixed(2),
                    y1:parseFloat(pc.y).toFixed(2),
                    x2:parseFloat(pc_end.x).toFixed(2),
                    y2:parseFloat(pc_end.y).toFixed(2)
                    // cx:parseInt(pc.x),
                    // cy:parseInt(pc.y)
                }
                data.push(d);
             }
             return data;
        }
    },

    methods:{
         svg:function(){
            // svg needs cleaning:
            let svgstring = this.$refs.svg.innerHTML;
            svgstring = svgstring.replace('projector.click+','');
            return `<svg width="${this.width}px" height="${this.height}px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve">${svgstring}</svg>`;
        },
        shout:function(){

        },
        getColour:function(id){
            let timesover = Math.floor(id / this.colors.length);
            return this.colors[id - this.colors.length*timesover];
        },
        polarToCartesian :function(centerX, centerY, radius, angleInDegrees){
  	        var angleInRadians = (angleInDegrees-90) * Math.PI / 180.0;
        	return {
    	        x: centerX + (radius * Math.cos(angleInRadians)),
    	        y: centerY + (radius * Math.sin(angleInRadians))
  	        };
        }
    }
    

}
</script>