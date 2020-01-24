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

    <img ref="previewImg" :src="svgcode" class="image-preview"/>

    <!-- <p>Here's a div with CSS variable used in background</p> -->
    <div class="preview" ref="preview"></div>

    
    <div class="ui-wrapper">       
      <div class="ui">
        <div class="ui-primary">
          <Dragbar :rangeout="{min:1,max:24}" :step="1"  :decimals="0" :default="7" label="Number of elements" @update="updateHandler($event,'count');" class="ui-bar"/>
          <Dragbar :rangeout="{min:-48,max:48}" :step="1"  :decimals="0" :default="30" label="Radius"  @update="updateHandler($event,'radius');" class="ui-bar"/>
          <Dragbar :rangeout="{min:0.1,max:100}" :step=".1" :decimals="0"  :default="10" label="Element size" @update="updateHandler($event,'r');" class="ui-bar"/>
          <Dragbar :rangeout="{min:.5,max:100}" :step=".5" :decimals="1"  :default="8" label="Element thickness" @update="updateHandler($event,'thickness');" class="ui-bar"/>
          <Dragbar :rangeout="{min:0.1,max:10}" :step=".1" :decimals="2"  :default="5" label="Transition Time " @update="updateSpeed" class="ui-bar" />
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
      <p>Rainbow Spinner, is a side project that explores SVG as a self contained animation format. /p>
      <p>SMIL, animation markup for SVG allows easy access to animation parameters for customisation, while a CSS variable is an easy copy&amp;paste way to add a spinner to your project. What you get is an animted SVG, wrapped in a CSS variable. You can also download the animated SVG file.</p>      
      
      <p class="atp-sidenote">This website has been built with vue.js &amp; nuxt.js, 
        source code at <a href="https://github.com/tomekkozysa/svg-spinner">@github</a></p>

        <p>You can find me on <a href="https://twitter.com/tokya">twitter</a>, or here <a href="https://kozysa.me">https://kozysa.me</a>, would be great to find out how it's beeing used!</p>
      
      <!-- <p>
        https://developer.mozilla.org/en-US/docs/Web/SVG/SVG_animation_with_SMIL
        <quote>Although Chrome 45 deprecated SMIL in favor of CSS animations and Web animations, the Chrome developers have since suspended that deprecation.</quote>
        </p>       -->
      
    </div>
    <!-- it can very long, has to remain visible for copy to clipboard to work but it's hidden with CSS 
    -->

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
      radius:30,
      r:8,
      thickness:10,
      time:5,
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

    // this function Lucky was meant to generate random values, it createved several problems, hence suspension

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

   --svgicon-about: url('data:image/svg+xml;utf-8,<svg width="200px" height="200px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve"><g><line x1="100.00" y1="81.00" x2="100.00" y2="89.00" stroke-width="10" stroke="violet" stroke-linecap="round"><animate dur="5000ms" repeatCount="indefinite" attributeName="stroke" values="violet;indigo;blue;lightgreen;yellow;darkorange;red" begin="0ms"></animate></line><line x1="116.45" y1="109.50" x2="109.53" y2="105.50" stroke-width="10" stroke="indigo" stroke-linecap="round"><animate dur="5000ms" repeatCount="indefinite" attributeName="stroke" values="violet;indigo;blue;lightgreen;yellow;darkorange;red" begin="1666.6666666666667ms"></animate></line><line x1="83.55" y1="109.50" x2="90.47" y2="105.50" stroke-width="10" stroke="blue" stroke-linecap="round"><animate dur="5000ms" repeatCount="indefinite" attributeName="stroke" values="violet;indigo;blue;lightgreen;yellow;darkorange;red" begin="3333.3333333333335ms"></animate></line></g></svg>');
}
body{
  background: var(--c-body-bgr);
}



.container.fullscreenmode .image-preview{
  opacity: 0;
}
.container.fullscreenmode{
  background: var(--d-preview-bgr);
}

.container {
  
  margin: 0 auto;
  min-height: 100vh;
  padding-top:2em;
  display: flex;
  justify-content: center;
  align-items: center;
   font-family: -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 20px;
  color:#444;
  letter-spacing: 1px;
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
  margin:4em auto;
  height:0;overflow:hidden;opacity:0;
  width:90%;
  overflow: hidden;

  display: block;
  outline:none;
  border:0px;
  padding:1em;
  background: transparent;
  
}
.about-the-project{
  width:80%;
  max-width: 800px;
  margin:2em auto;
  padding-top:1em;
  border-top:4px solid black;
  line-height: 1.4em;
  padding-bottom:100px;
  background: var(--svgicon-about) no-repeat center 100%;
  background-size:100px;
    
  
}

.about-the-project p{
  margin:1em auto 0;
}

.about-the-project a{
  background:white;
  text-decoration: none;
  color:#333;
  display:inline-block;
  padding:0 .2em;
}
.about-the-project a:hover{
  text-decoration: underline;
  background: transparent;
}

p.atp-sidenote{
  border-top:1px solid black;
  padding:2em 0 0;
}
.ui{

  margin:0 auto;
  color:#333;
  display: flex;
  align-items: center;
  flex-direction:column;
   
}

.ui-bar{
  transition: background .3s;
}
.ui-buttons{
  display: flex;
  flex-direction: row;
}
.ui-button{
  margin:0 .25em;
}

.ui-primary{
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
  display:none;
  background: transparent var(--d-preview-bgr);
  /* border:1px solid red; */
  margin:2em auto;
  width:200px;
  height:200px;
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
   background:white;

}
.action-default.hold{
   background:white;
}
</style>
