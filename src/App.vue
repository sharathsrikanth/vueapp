<template>
 <div class="row">
     
    <div class="col-sm-5" >

      <h3 id="heading"> Emotion analyzer </h3>

      <edi-tor  v-on:listenchange="plotgraphbool($event)" :mark1="markemotion" v-on:suggestion="plotsuggestion($event)"> </edi-tor>
    </div>
    
    <div class="col-sm-3" >
      
       <rest-calls  :myProp1="text" :myProp2="emotionvalue" :myProp3="suggestionword" v-on:abc="listen($event)" v-on:listentographvalue="plotgraph($event)" v-on:listentovalencevalue="plotvalence($event)"  v-on:listentomarkvalue="marksentence($event)"  > </rest-calls>

      <horizontal-barchart  v-on:listentoemotionclicked="markup($event)" v-on:listensuccess="this.plot='true'" :data="graphvalue" id="horizontalbarchart"  ></horizontal-barchart>

      <line-chart  v-on:listensuccess="this.plot='true'" :data="valencevalue" id="valencechart"  ></line-chart>

    </div>   

    <div class="col-sm-4">
         <te-mp :data="suggestiondata" :label="suggestionlabel" id="barchart"  ></te-mp>
         <p id="suggestionword">Word : {{this.suggestionword}} </p>
    </div> 

 </div> 
</template>

<script>
import Horizontalbarchart from './components/barchart'
import Linechart from './components/linechart'
import Editor from './components/Editor'
import Restcalls from './components/restcalls'
import temp from './components/temp'


export default {
  name: 'app',
   components:{
      'edi-tor':Editor,
      'horizontal-barchart':Horizontalbarchart,
      'line-chart':Linechart,
      'rest-calls':Restcalls,
      'te-mp':temp
  },
  data () {
    return {
      suggestionword:'',
      suggestiondata:'',
      suggestionlabel:'',
      text:'',
      graphvalue:'',
      valencevalue:'',
      emotionvalue:'',
      markemotion:'',
      plot:'false'
    }
  },
  methods: {
    plotsuggestion : function(val) {
       this.suggestionword=val
    },
    listen : function(val){
      this.suggestiondata=val.data
      this.suggestionlabel=val.label
    },
    marksentence : function(val){
      this.markemotion=val
    },
    markup : function(value) {    
      this.emotionvalue=value
    },
    plotgraphbool : function(val) {
      this.text= val;
      console.log(this.text.article);
    },
    plotgraph: function(val) {
     this.graphvalue=val
     
    },
    plotvalence: function(val){
     this.valencevalue=val
     alert(valencevalue);
    }
    
  }
 

}
</script>

<style>
  
  .row{
    background-color:#333;
  }

  #heading{
   margin: 25px;
   color:#dd4814;
   position:absolute;
  }

  #horizontalbarchart{
    max-width: 300px;
    height:300px;
    margin:  150px auto;
    margin-top:50px;
  }

  #barchart{
    max-width: 300px;
    height:300px;
    margin:  150px auto;
    margin-top:100px;
  }

  #valencechart{
    max-width: 200px;
    height:300px;
    margin:  150px auto;
   
  }

  #age{
   margin-top: 45px;
   font-size: 14px;
  }

 #totalwords{
   font-size: 14px;
  }

  #avg{
   font-size: 14px;
  }

  #formality{
   font-size: 14px;
  }
  
  #suggestionword{
    color:#fff;
    margin-left:160px;
    margin-top:-120px;
  }
  

</style>
