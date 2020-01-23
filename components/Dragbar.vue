<template>
    <div ref="drag" class="dragbar" :class={debug:debug}>

        <div class="dragbar-label">
            <span class="dragbar-label-text" >{{label}}</span>
            <span class="dragbar-label-value" >{{precision}}</span>
        </div>
      
        <div class="dragbar-head">
            <div class="dragbar-trackbar-dot" :style="headmove"></div>
            <div class="dragbar-trackbar-wrapper">                
                <div class="dragbar-trackbar" :style="headmove"></div>
                <div class="dragbar-trackbar-track"></div>
        </div>

        <small v-if="debug">
            {{percentage}}% <br />
            range:{{ rangeout.max - rangeout.min }}<br />
        </small>
       

    </div>
    </div>
</template>
<script>
/*
- try use movementX - how many steps of change in rangein, 


*/



export default {
    name:'Dragbar',
    props:{
        rangeout:Object,
        default:Number,
        label:String,
        step:Number,
        debug:Boolean,
        decimals:Number,
    },
    data(){
        return{
            
            is_mousedown:false,        
            width:200,
            rangein:{min:0,max:200},   
            move_distance:0,
            // step_count:this.step,
            value:this.default,
            
            
        }
    },
    computed:{

        precision:function(){
           return parseFloat(this.value).toFixed(this.decimals);
        },
        headmove:function(){
            let dist = -200 + (200*this.percentage)/100;
            return `transform:translateX(${dist}px)`
        },
        threshold:function(){
            return (this.rangein.max - this.rangein.min) /( this.rangeout.max - this.rangeout.min)
        },
        percentage:function(){
            return ( this.value  - this.rangeout.min )  * 100 / (this.rangeout.max - this.rangeout.min) ;
        },
        step_count:function(){

            return ( this.rangeout.max - this.rangeout.min ) / this.step;

        },
        moveby:function(){
            let range_value =  (this.rangein.max - this.rangein.min );
            let step_in_value = range_value/this.step_count;
            
            let value =  ((this.move_distance  / step_in_value) * this.step);

            // return Math.ceil(value);
            return value;
            // return this.range(this.move_distance  /((this.rangein.max - this.rangein.min) / this.step_count) ) ;
        },
    },
    mounted:function(){
            this.$refs.drag.addEventListener('mousedown',this.mousedown);         
    },
    watch:{
        // default:function(val){
        //     if(this.is_mousedown || this.value==val){
        //         return;
        //     }
        //     else{
        //         this.value=val;
        //         setTimeout(()=>{
        //             this.$emit('update',this.precision);
        //         },100)

        //     }
            
        // }
    },
    methods:{
        
        range:function(value){
            let newvalue  = (value - this.rangein.min) * (this.rangeout.max - this.rangeout.min) / (this.rangein.max - this.rangein.min) + this.rangeout.min;
            
                return newvalue;
            
        },
       
        update:function(updateValue){
            // let updateValue = this.range(this.move_distance);

            // let change = this.range(this.move_distance);
            // this.value+= change;
            this.value+= this.moveby;
            // console.log(change)

            if(this.value > this.rangeout.max){
                this.value = this.rangeout.max;
            }
            else if(this.value < this.rangeout.min){
                this.value = this.rangeout.min;
            }

            // this.value = parseFloat(this.value).toFixed(this.decimals);
            this.$emit('update',this.precision);
            // console.log('update',this.precision);
            
        },
        mousemove: function(ev){
            // console.log(ev.target)
            this.move_distance=ev.movementX;
            // this.update(ev.movementX * this.threshold);
            this.update(ev.movementX);
        },
        mouseup:function(){
            this.is_mousedown = false;
            this.move_distance = 0;
            // this.$refs.drag.removeEventListener('mousemove',this.mousemove);
            window.removeEventListener('mousemove',this.mousemove);
            window.removeEventListener('mouseup',this.mouseup);
        },
        mousedown:function(){
            this.is_mousedown = true;
            // this.$refs.drag.addEventListener('mousemove',this.mousemove);
            window.addEventListener('mousemove',this.mousemove);
            window.addEventListener('mouseup',this.mouseup);
        },
        
    
    }
}

</script>
<style>
.dragbar{


    --fs-label:14px;
    --fw-label:normal;
    
    --fs-value:18px;
    --fw-value:normal;

    --width:200px;
    --min-height:20px;

    --tb-bgr:#999;
    --tb-height:1px;

    --tb-dot-bgr:#999;
    --tb-dot-size:5px;
    

    --tb-track-bgr:#fff;
    

    

    user-select: none;
    width:var(--width);
    min-height:var(--min-height);
    text-align:left;
    position: relative;    
    cursor:ew-resize;
}
.dragbar:hover{
    --tb-dot-size:9px;
    --tb-bgr:black;
    --tb-dot-bgr:black;
}
.dragbar.debug{
        --min-height:120px;
}
small{
    font-size:.7em;
}
/* .dragbar:hover{
    --tb-bgr:yellow;
    --tb-height:3px;

    --tb-dot-bgr:yellow;
    
} */

.dragbar-label{
    position: relative;
    z-index:10;
    display: flex;
    flex-direction: row;
    letter-spacing: 0;

}
.dragbar-label-text{
    flex:2;
    font-weight: var(--fw-label);
    font-size:var(--fs-label);
   
    
}
.dragbar-label-value{
      font-weight: var(--fw-value);
    font-size:var(--fs-value);
    align-self: flex-end;    
}

.dragbar-head{
    height:100%;
    width:100%;
    position: relative;
    min-height:10px;
    top:0;
    left:0;

}
.dragbar-trackbar-wrapper{
    height:100%;
    width:100%;
    overflow: hidden;
    position: absolute;
    display: flex;
    align-content: center;
    align-items: center;
     top:calc(50% - calc(var(--tb-height) / 2));

}
.dragbar-trackbar{
    height:var(--tb-height);
    width:100%;
    background:var(--tb-bgr);
    top:0;
    left:0;
    z-index: 1;
    position: absolute;
    /* transition: all .1s; */
}
.dragbar-trackbar-track{
    position: absolute;
    height:var(--tb-height);
    width:100%;
    background:var(--tb-track-bgr);
    top:0;
    left:0;
    z-index: 0;

}
.dragbar-trackbar-dot{
    
    height:var(--tb-dot-size);
    width:var(--tb-dot-size);
    transition: width .1s, height .1s;
    border-radius:999em;
    background:var(--tb-dot-bgr);
    position:absolute;
    top:calc(50% - calc(var(--tb-dot-size) / 2));
    right:calc(var(--tb-dot-size) / -2);
    z-index:10;
    

}

</style>