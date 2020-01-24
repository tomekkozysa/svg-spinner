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

    <!-- 
    
      this image uses svg code to display the spinner. 
      I decided to use image as it refreshes the changes quickly.
      Maniuplating SVG directly was too slow and was getting confused when maniuplating parameters.
      CSS var should have about same refresh time as image.


    -->

    <!-- <img ref="previewImg" :src="svgcode" class="image-preview"/> -->

    <!-- <p>Here's a div with CSS variable used in background</p> -->
    <div class="preview" ref="preview"></div>

    
    <div class="ui-wrapper">       
      <div class="ui">
        <div class="ui-primary">
          <Dragbar :rangeout="{min:1,max:24}" :step="1"  :decimals="0" :default="7" label="Number of elements" @update="updateHandler($event,'count');" class="ui-bar"/>
          <Dragbar :rangeout="{min:-48,max:48}" :step="1"  :decimals="0" :default="9" label="Radius"  @update="updateHandler($event,'radius');" class="ui-bar"/>
          <Dragbar :rangeout="{min:0.1,max:100}" :step=".1" :decimals="0"  :default="3" label="Size" @update="updateHandler($event,'r');" class="ui-bar"/>
          <Dragbar :rangeout="{min:.5,max:100}" :step=".5" :decimals="1"  :default="2" label="Thickness" @update="updateHandler($event,'thickness');" class="ui-bar"/>
          <Dragbar :rangeout="{min:0.1,max:10}" :step=".1" :decimals="2"  :default="2.25" label="Transition Time " @update="updateSpeed" class="ui-bar" />
        </div>

        <div class="ui-secondary ui-buttons">
          <button @click="copyCSS" class="action-default  ui-button">Copy CSS code</button>
          <button @click="exportAsSVG" class="action-default  ui-button" >Download SVG</button>
          <button @click="(e)=>fullscreenmode=!fullscreenmode" class="action-default  ui-button" :class="{hold:fullscreenmode}">Hit me!</button>
          <!-- <button @click="lucky" class="action-default">Feeling lucky!</button> -->
        </div>
      </div>
      <div class="svg-bytes-value" >~{{svgcode | bytes}}</div>
    </div>

    <div class="about-the-project">
      <p>Rainbow Spinner, is a side project that explores SVG as a self contained animation format. </p>
      <p>As SVG allows easy access to animation parameters for customisation, a CSS variable is an easy copy&amp;paste way to add a spinner to your project.</p>      
      <p>You can find me on <a href="https://twitter.com/tokya">twitter</a>, or here <a href="https://kozysa.me">https://kozysa.me</a>, would be great to see how you'd use it!</p>
      
      <p>This website has been built with vue &amp; nuxt.js., 
        source <a href="https://github.com/tomekkozysa/svg-spinner">@github</a></p>
      <!-- <p>
        https://developer.mozilla.org/en-US/docs/Web/SVG/SVG_animation_with_SMIL
        <quote>Although Chrome 45 deprecated SMIL in favor of CSS animations and Web animations, the Chrome developers have since suspended that deprecation.</quote>
        </p>       -->
      
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
      if(value % 1 !== 0){
        return parseFloat(value).toFixed(2);  
      }
      else{

        return parseInt(value);
      }
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
      time:2.25,
      dotspeed:.25,
      svgcode:'',
      images:'',
      fullscreenmode:false,
    }
  },
  mounted:function(){
    this.updatePreview();
  },
  computed:{},
  methods:{


    triggerDownload : function (imgURI,name) {
        var evt = new MouseEvent('click', {
            view: window,
            bubbles: false,
            cancelable: true
        });
        var a = document.createElement('a');
        a.setAttribute('download', name);
        a.setAttribute('href', imgURI);
        a.setAttribute('target', '_blank');
        a.dispatchEvent(evt);

    },    
    exportAsSVG :  function () {
      
      var url = "data:image/svg+xml;charset=utf-8,"+encodeURIComponent(this.$refs.projector.svg());
      this.triggerDownload(url,'rainbow-spinner-'+Math.random()*10000+'.svg');

    },


    toNumber: function (value) {
      if(value % 1 !== 0){
        // console.log('toNumber:float',value)
        return parseFloat(value).toFixed(2);  
      }
      else{
        // console.log('toNumber:int',value)
        return parseInt(value);
      }
    },
    // lucky:function(){
      
    //     this.count = 1+Math.random()*23;
    //     this.radius = Math.random()*48 - Math.random()*48;
    //     this.r = Math.random()*36;
    //     this.thickness = Math.random()*25;
    //     this.time = Math.random()*1 + Math.random()*5;
    //     this.dotspeed = Math.random()*10/10;
       
    // },
    updateHandler(value,type){
      this[type] = this.toNumber(value);
      this.updatePreview();
    },
    updateSpeed(value,type){
      this.time = this.toNumber(value);
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
   --c-body-bgr:#f5f4e9;
}
body{
  background: var(--c-body-bgr);
}



.container.fullscreenmode .image-preview{
  opacity: 0;
}
.container.fullscreenmode{
 /* --svgicon:''; */
  background: var(--d-preview-bgr);
}
.image-preview{
  margin:0 auto;
  width:200px;
  display:block;
}
.svg-bytes-value{
  margin-top:20px;
  text-align: center;
}
.code{
  font-size:11px;
  opacity: .8;
  font-family:monospace;
  /* white-space: nowrap; */
  /* padding:20px; */
  /* max-width:300px; */
  margin:4em auto;
  height:0;overflow:hidden;opacity:0;
  width:90%;
  /* height:300px; */
  overflow: hidden;

  display: block;
  outline:none;
  border:0px;
  padding:1em;
  background: transparent;
  /* display: none; */
}
.about-the-project{
  width:80%;
  max-width: 800px;
  margin:4em auto;
  padding-top:1em;
  border-top:4px solid black;
  line-height: 1.4em;
}

.about-the-project a{
  background:white;
  /* border-bottom:2px solid black; */
text-decoration: none;
color:#333;
}

.ui{

  

  margin:0 auto;
  color:#333;
  display: flex;
  align-items: center;
  flex-direction:column;
  
  
}

.ui-bar{
  margin-top:20px; 
  transition: background .3s;
}
.ui-buttons{
  display: flex;
  flex-direction: row;
}
.ui-button{
  margin:0 .25em;
}
/* .ui-bar:hover{
  background:#eee;
} */
ui-primary{
  width:280px;
  padding:40px;
  background: var(--c-body-bgr);
  
}
.ui-secondary{
  margin-top:2em;
  text-align: center;
   padding:40px;
  background: var(--c-body-bgr);
}
.projector{
  display:none;
}
.preview{
  /* display:none; */
  background: transparent var(--d-preview-bgr);
  /* border:1px solid red; */
  margin:2em auto;
  width:200px;
  height:200px;
}


.container {
  /* background:black; */
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  /* text-align: center; */
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
  background:var(--c-body-bgr);
  
  border-radius:2px;
  outline:none;
  border:1px solid #999;
  font-size:14px;
  padding:.5em 1em;
  cursor:pointer;
transition: background .3s;
/* background: transparent var(--d-preview-bgr) no-repeat center; */
}
.action-default:not(.hold):hover{
   /* background: white; */
   /* background:var(--d-preview-bgr) no-repeat center; */
   /* background-size:100px; */
   /* color:rgba(0,0,0,0) */
   background:white;

}
.action-default.hold{

   background:white;
}
</style>
