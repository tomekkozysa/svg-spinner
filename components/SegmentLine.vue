<template>
 <line :x1="x1" :y1="y1" :x2="x2" :y2="y2" :stroke-width="size" :stroke="colour" stroke-linecap="round">
    <slot />
  </line>
</template>

<script>
export default {


    props:{

        r:Number,
        x1:String,
        y1:String,
        x2:String,
        y2:String,
        colour:String,
        size:Number,
    }
    

}
</script>