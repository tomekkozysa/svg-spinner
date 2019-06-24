<template>
  <circle :cx="cx" :cy="cy" :r="r" :fill="fill">
    <slot />
  </circle>
</template>

<script>
export default {


    props:{
        r:Number,
        cx:String,
        cy:String,
        fill:String,
    }
    

}
</script>