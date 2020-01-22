<template>
  <div class="container" :class="{fullscreenmode:fullscreenmode}">
    <Projector ref="projector" class="projector"
      :width=200 :height=200 
      :count="count | num " 
      :radius="radius | num " 
      :time="time | num" 
      :r="r | num " 
      :thickness="thickness | num " 
      :dotspeed=".1 "/>   



    <img ref="previewImg" :src="svgcode" class="image-preview"/>
    <!-- <img v-for="img in images" ref="previewImg" :src="img.svgcode" /> -->

    <!-- <p>Here's a div with CSS variable used in background</p> -->
    <div class="preview" ref="preview">
      
    </div>

    
     <div>
       <div class="ui">
      <Dragbar :rangeout="{min:2,max:24}" :step="1"  :decimals="0" :default="count" label="Number of elements" @update="updateHandler($event,'count');" class="ui-bar"/>
      <Dragbar :rangeout="{min:-48,max:48}" :step="1"  :decimals="0" :default="radius" label="Radius"  @update="updateHandler($event,'radius');" class="ui-bar"/>
      <Dragbar :rangeout="{min:0.1,max:48}" :step=".1" :decimals="0"  :default="r" label="Size" @update="updateHandler($event,'r');" class="ui-bar"/>
      <Dragbar :rangeout="{min:.5,max:100}" :step=".5" :decimals="1"  :default="thickness" label="Thickness" @update="updateHandler($event,'thickness');" class="ui-bar"/>
      <Dragbar :rangeout="{min:0.1,max:10}" :step=".1" :decimals="2"  :default="time" label="Transition Time " @update="updateSpeed" class="ui-bar" />
    </div>

    <div class="ui-secondary">
      
     
        <button @click="copyCSS" class="action-default">Copy CSS</button>
        <button @click="(e)=>fullscreenmode=!fullscreenmode" class="action-default">Hit me!</button>
        <button @click="lucky" class="action-default">Feeling lucky!</button>
      </div>

       <div class="svg-bytes-value" >~{{svgcode | bytes}}</div>

    </div>

    
    <textarea class="code" ref="csscode">background: var(--svgicon);
--svgicon: url('{{svgcode}}');
    </textarea>

    </div>
</template>

<script>
  import Projector from '~/components/Projector.vue'
  import Dragbar from '~/components/Dragbar.vue'

export default {
  
  components: {
    Projector,
    Dragbar
  },

  filters: {
    num: function (value) {
      if(value < 1 ){
      return parseFloat(value).toFixed(2)  
      }
      console.log('num::',value)
      return parseInt(value);
    },
      bytes:function(val){
        // https://stackoverflow.com/questions/2219526/how-many-bytes-in-a-javascript-string
      return (encodeURI(val).split(/%..|./).length - 1)/1000+'kb';

      },
   
    
},
  data(){
    return{
      count:7,
      radius:9,
      r:3,
      thickness:2,
      time:.25,
      dotspeed:.25,
      svgcode:'',
      images:'',
      fullscreenmode:false,
    }
  },
  mounted:function(){
    this.updatePreview();
  },
  watch:{
    // count:function(){ this.updatePreview(); console.log('count')},
    radius:function(){ this.updatePreview();},
    r:function(){ this.updatePreview();},
  },
  computed:{},
  methods:{
    lucky:function(){
      
        this.count = 1+Math.random()*23;
        this.radius = Math.random()*48 - Math.random()*48;
        this.r = Math.random()*36;
        this.thickness = Math.random()*25;
        this.time = Math.random()*1 + Math.random()*5;
        this.dotspeed = Math.random()*10/10;
    },
    updateHandler(value,type){
      this[type] = value;
      this.updatePreview();
    },
    updateSpeed(value,type){
      this.time = value;
      this.updatePreview();
    },
    updatePreview:function(){
        this.svgcode = 'data:image/svg+xml;utf-8,'+this.$refs.projector.svg();
        document.documentElement.style.setProperty('--d-preview-bgr', "url('"+this.svgcode+"')");
        // document.documentElement.style.setProperty('--svgicon', "url('"+this.svgcode+"')");
        
    },
    copyCSS:function(){
      const el = this.$refs.csscode;
  el.select();
  document.execCommand('copy');
    }
  }
  
}


</script>
<style>
:root {
  --d-preview-bgr:'';
  /* //url('data:image/svg+xml;utf-8,<svg width="200px" height="200px" id="projector" xmlns="http://www.w3.org/2000/svg"><g><circle cx="100" cy="81" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0s"></animate></circle><circle cx="106" cy="82" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.1111111111111111s"></animate></circle><circle cx="112" cy="85" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.2222222222222222s"></animate></circle><circle cx="116" cy="90" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.3333333333333333s"></animate></circle><circle cx="118" cy="96" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.4444444444444444s"></animate></circle><circle cx="118" cy="103" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.5555555555555556s"></animate></circle><circle cx="116" cy="109" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.6666666666666666s"></animate></circle><circle cx="112" cy="114" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.7777777777777777s"></animate></circle><circle cx="106" cy="117" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.8888888888888888s"></animate></circle><circle cx="100" cy="119" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1s"></animate></circle><circle cx="93" cy="117" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.1111111111111112s"></animate></circle><circle cx="87" cy="114" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.222222222222222s"></animate></circle><circle cx="83" cy="109" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.3333333333333333s"></animate></circle><circle cx="81" cy="103" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.4444444444444444s"></animate></circle><circle cx="81" cy="96" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.5555555555555554s"></animate></circle><circle cx="83" cy="90" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.6666666666666665s"></animate></circle><circle cx="87" cy="85" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.7777777777777777s"></animate></circle><circle cx="93" cy="82" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.8888888888888888s"></animate></circle></g></svg>'); */
  /* --d-ico-closex:url('data:image/svg+xml;utf8,<svg width="50px" height="50px" xmlns="http://www.w3.org/2000/svg"><g><g transform="scale(.5, 25, 25)"><line x1="0" y1="0" x2="50" y2="50" stroke="black" stroke-width="1" /><line x1="0" y1="50" x2="50" y2="0" stroke="black" stroke-width="1" /></g></g></svg>'); */
  /* --d-ico-closex:url('data:image/svg+xml;utf8,<svg width="200px" height="200px" xmlns="http://www.w3.org/2000/svg"><circle cx="100" cy="91" r="3" fill="red" /></svg>'); */
}
body{
  background:  #f5f4e9;
}



.container.fullscreenmode .image-preview{
  opacity: 0;
}
.container.fullscreenmode{
 /* --svgicon:''; */
  background: var(--d-preview-bgr);
}
.svg-bytes-value{
  margin-top:20px;
}
.code{
  font-size:11px;
  opacity: .8;
  font-family:monospace;
  /* white-space: nowrap; */
  /* padding:20px; */
  /* max-width:300px; */
  margin:4em auto;
  
  width:90%;
  height:300px;
  overflow: hidden;

  display: block;
  outline:none;
  border:0px;
  padding:1em;
  background: transparent;
  /* display: none; */
}
.ui{
  margin:0 auto;
  width:220px;
}

.ui-bar{
  margin-top:20px;
  /* padding:10px; */
  transition: background .3s;
}
/* .ui-bar:hover{
  background:#eee;
} */

.ui-secondary{
  margin-top:4em;
  
}
.projector{
  display:none;
}
.preview{
  display:none;
  background: rgb(25,25,35) var(--d-preview-bgr);
  border:1px solid red;
  margin:3em auto;
  width:200px;
  height:200px;
}
input[type="range"]{
  width:450px;

}
.container {
  /* background:black; */
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
   font-family: -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 20px;
  /* color: #35495e; */
  color:#444;
  letter-spacing: 1px;
}

.action-default{
  background:transparent;
  /* border:1px; */
  border-radius:0;
  outline:none;
  border:1px solid black;
  font-size:16px;
  padding:.5em 1em;
  cursor:pointer;
/* transition: background .3s; */
/* background: transparent var(--d-preview-bgr) no-repeat center; */
}
.action-default:hover{
   /* background: white; */
   background:var(--d-preview-bgr) no-repeat center;
   background-size:100px;
   color:rgba(0,0,0,0)
}
</style>
