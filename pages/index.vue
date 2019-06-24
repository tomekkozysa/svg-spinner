<template>
  <div class="container">
    <Projector ref="projector"
      :width=200 :height=200 
      :count="count | num " :radius="radius | num " :time="time | num " :r="r | num " :dotspeed=".1 | num"/>



<!-- <img src='data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="Layer_1" x="0px" y="0px" viewBox="0 0 100 100" enable-background="new 0 0 100 100" xml:space="preserve" height="100px" width="100px">
<g>
    <path d="M28.1,36.6c4.6,1.9,12.2,1.6,20.9,1.1c8.9-0.4,19-0.9,28.9,0.9c6.3,1.2,11.9,3.1,16.8,6c-1.5-12.2-7.9-23.7-18.6-31.3   c-4.9-0.2-9.9,0.3-14.8,1.4C47.8,17.9,36.2,25.6,28.1,36.6z"/>
    <path d="M70.3,9.8C57.5,3.4,42.8,3.6,30.5,9.5c-3,6-8.4,19.6-5.3,24.9c8.6-11.7,20.9-19.8,35.2-23.1C63.7,10.5,67,10,70.3,9.8z"/>
    <path d="M16.5,51.3c0.6-1.7,1.2-3.4,2-5.1c-3.8-3.4-7.5-7-11-10.8c-2.1,6.1-2.8,12.5-2.3,18.7C9.6,51.1,13.4,50.2,16.5,51.3z"/>
    <path d="M9,31.6c3.5,3.9,7.2,7.6,11.1,11.1c0.8-1.6,1.7-3.1,2.6-4.6c0.1-0.2,0.3-0.4,0.4-0.6c-2.9-3.3-3.1-9.2-0.6-17.6   c0.8-2.7,1.8-5.3,2.7-7.4c-5.2,3.4-9.8,8-13.3,13.7C10.8,27.9,9.8,29.7,9,31.6z"/>
    <path d="M15.4,54.7c-2.6-1-6.1,0.7-9.7,3.4c1.2,6.6,3.9,13,8,18.5C13,69.3,13.5,61.8,15.4,54.7z"/>
    <path d="M39.8,57.6C54.3,66.7,70,73,86.5,76.4c0.6-0.8,1.1-1.6,1.7-2.5c4.8-7.7,7-16.3,6.8-24.8c-13.8-9.3-31.3-8.4-45.8-7.7   c-9.5,0.5-17.8,0.9-23.2-1.7c-0.1,0.1-0.2,0.3-0.3,0.4c-1,1.7-2,3.4-2.9,5.1C28.2,49.7,33.8,53.9,39.8,57.6z"/>
    <path d="M26.2,88.2c3.3,2,6.7,3.6,10.2,4.7c-3.5-6.2-6.3-12.6-8.8-18.5c-3.1-7.2-5.8-13.5-9-17.2c-1.9,8-2,16.4-0.3,24.7   C20.6,84.2,23.2,86.3,26.2,88.2z"/>
    <path d="M30.9,73c2.9,6.8,6.1,14.4,10.5,21.2c15.6,3,32-2.3,42.6-14.6C67.7,76,52.2,69.6,37.9,60.7C32,57,26.5,53,21.3,48.6   c-0.6,1.5-1.2,3-1.7,4.6C24.1,57.1,27.3,64.5,30.9,73z"/>
</g>
</svg>' alt="" /> -->

    

    <img ref="previewImg" :src="svgcode" />
    <!-- <img v-for="img in images" ref="previewImg" :src="img.svgcode" /> -->

    <div class="preview" ref="preview">

    </div>

    
     <div>
      <div> 
        <span>Count: {{count}}</span><br />
        <input type="range" name="count" @change="updatePreview"
         min="3" max="64" v-model="count">
      </div>

      <div> 
        <span>Radius: {{radius}}</span><br />
        <input type="range" name="radius" @change="updatePreview"
         min="3" max="48" v-model="radius">
      </div>
      
      <div> 
        <span>Dot radius: {{r}}</span><br />
        <input type="range" name="r" @change="updatePreview"
         min="1" max="12" v-model="r">
      </div>
      
      <div> 
        <span>Time: {{time}}</span><br />         
        <input type="range" name="time" @change="updatePreview"
         min=".1" max="10" step=".1" v-model="time">
      </div>   
      
      
    </div>
    <!-- <svg width="200" height="200" id="projector" xmlns="http://www.w3.org/2000/svg"><g><circle cx="100" cy="91" r="3" fill="#ff0000"></circle><circle cx="107.03648334221226" cy="94.3885917832714" r="3" fill="#ff7f00"></circle><circle cx="108.77435120963641" cy="102.00268840560683" r="3" fill="#ffff00"></circle><circle cx="103.90495365205803" cy="108.10871981112177" r="3" fill="#00ff00"></circle><circle cx="96.09504634794197" cy="108.10871981112177" r="3" fill="#0000ff"></circle><circle cx="91.22564879036359" cy="102.00268840560683" r="3" fill="#4b0082"></circle><circle cx="92.96351665778774" cy="94.3885917832714" r="3" fill="#9400d3"></circle></g></svg> -->
  </div>
</template>

<script>
import Projector from '~/components/Projector.vue'
export default {
  components: {
    Projector
  },
  filters: {
    num: function (value) {
      if(value < 1 ){
      return parseFloat(value).toFixed(2)  
      }
      return parseInt(value)
    },
},
  data(){
    return{
      count:7,
      radius:9,
      r:3,
      time:1,
      dotspeed:.25,
      svgcode:'',
      images:'',
    }
  },
  mounted:function(){
    this.updatePreview();
  },
  computed:{},
  methods:{
    updatePreview:function(){
        this.svgcode = 'data:image/svg+xml;utf-8,'+this.$refs.projector.svg();
        // this.images.push[] = 'data:image/svg+xml;utf-8,'+this.$refs.projector.svg();
        // this.$forceUpdate();
        // this.$refs.previewImg.setAttribute("src",+this.svgcode)
        // console.log(svgcode);
        document.documentElement.style.setProperty('--d-preview-bgr', "url('"+this.svgcode+"')");
        // this.$refs.preview.style.backgroundImage = "";
        // this.$refs.preview.style.backgroundImage = "url('"+this.svgcode+"')";
        // console.log(getComputedStyle(document.documentElement).getPropertyValue('---preview-bgr')); 

    }
  }
  
}


</script>
<style>
:root {
  --d-preview-bgr:'';
  /* //url('data:image/svg+xml;utf-8,<svg width="200px" height="200px" id="projector" xmlns="http://www.w3.org/2000/svg"><g><circle cx="100" cy="81" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0s"></animate></circle><circle cx="106" cy="82" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.1111111111111111s"></animate></circle><circle cx="112" cy="85" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.2222222222222222s"></animate></circle><circle cx="116" cy="90" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.3333333333333333s"></animate></circle><circle cx="118" cy="96" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.4444444444444444s"></animate></circle><circle cx="118" cy="103" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.5555555555555556s"></animate></circle><circle cx="116" cy="109" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.6666666666666666s"></animate></circle><circle cx="112" cy="114" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.7777777777777777s"></animate></circle><circle cx="106" cy="117" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="0.8888888888888888s"></animate></circle><circle cx="100" cy="119" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1s"></animate></circle><circle cx="93" cy="117" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.1111111111111112s"></animate></circle><circle cx="87" cy="114" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.222222222222222s"></animate></circle><circle cx="83" cy="109" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.3333333333333333s"></animate></circle><circle cx="81" cy="103" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.4444444444444444s"></animate></circle><circle cx="81" cy="96" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.5555555555555554s"></animate></circle><circle cx="83" cy="90" r="4" fill="rgba(255,0,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.6666666666666665s"></animate></circle><circle cx="87" cy="85" r="4" fill="rgba(0,255,0,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.7777777777777777s"></animate></circle><circle cx="93" cy="82" r="4" fill="rgba(0,0,255,1)"><animate dur="2s" repeatCount="indefinite" attributeName="fill" values="rgba(255,0,0,1);rgba(0,255,0,1);rgba(0,0,255,1)" calcMode="splines" begin="1.8888888888888888s"></animate></circle></g></svg>'); */
  --d-ico-closex:url('data:image/svg+xml;utf8,<svg width="50px" height="50px" xmlns="http://www.w3.org/2000/svg"><g><g transform="scale(.5, 25, 25)"><line x1="0" y1="0" x2="50" y2="50" stroke="black" stroke-width="1" /><line x1="0" y1="50" x2="50" y2="0" stroke="black" stroke-width="1" /></g></g></svg>');
  --d-ico-closex:url('data:image/svg+xml;utf8,<svg width="200px" height="200px" xmlns="http://www.w3.org/2000/svg"><circle cx="100" cy="91" r="3" fill="red" /></svg>');
}
#projector{
  display:none;
}
.preview{
  
  background: rgb(25,25,35) var(--d-preview-bgr);
  border:1px solid red;
  margin:3em auto;
  width:200px;
  height:200px;
}
input[type="range"]{
  width:450px;

}
.container{
  background:white;
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

</style>
