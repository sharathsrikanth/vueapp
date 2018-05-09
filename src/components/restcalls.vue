<template>

   <div id="data">

    <div id = "load" >
    <font-awesome-icon icon="spinner" />
     <i v-if="this.loading === 'true'" > Analyzing . . .</i>

     </div>

      <div  id="age" v-if="this.write === 'true'">
      <p> Click here to know the article similar to current article </p> </br>
       <button type="button" class="btn " id="comparision">similarity</button>
    </div>

     <div  id="age" v-if="this.write === 'true'">
      <p>{{this.readability[0]}} </p> </br>
    </div>
    
   

     <div  id="totalwords" v-if="this.write === 'true'">
      <p>{{this.readability[1]}}</p> </br>
    </div>  

    <div  id="totalwords" v-if="this.write === 'true'">
      <p>{{this.readability[2]}}</p> </br>
    </div>  

    <div  id="totalwords" v-if="this.write === 'true'">
      <p>{{this.readability[3]}}</p> </br>
    </div>

    <div  id="totalwords" v-if="this.write === 'true'">
      <p>{{this.readability[4]}}</p> </br>
    </div> 

    <div id="avg" v-if="this.write === 'true'">
      <p>{{this.readability[5]}}</p> </br>
    </div>

     <div id="formality" v-if="this.write === 'true'" >
      <p>{{this.readability[6]}}</p> </br>

    </div>

     <div id="emotionbuttons" v-if="this.write === 'true'">
        <div class="btn-group-vertical btn-group-sm" id="1">
         <button v-on:click="markupsentece(0)" type="button" class="btn " id="anger">Anger</button>
         <button v-on:click="markupsentece(1)" type="button" class="btn " id="anticipation">Anticipation</button>
         <button v-on:click="markupsentece(2)" type="button" class="btn " id="disgust" >Disgust</button>
         <button v-on:click="markupsentece(3)" type="button" class="btn " id="fear">Fear</button>
         </div>

         <div class="btn-group-vertical btn-group-sm" id="2">
         <button v-on:click="markupsentece(4)" type="button" class="btn " id="joy">Joy</button>
         <button v-on:click="markupsentece(5)" type="button" class="btn " id="sadness">Sadness</button>
          <button v-on:click="markupsentece(6)" type="button" class="btn " id="suprise">Suprise</button>
         <button v-on:click="markupsentece(7)" type="button" class="btn " id="Trust">Trust</button>
        </div>
      </div> 
   </div>
</template>
<script>


import axios from 'axios';
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'

export default {
  props: [ 'myProp1' , 'myProp2' ,'myProp3'],
  components: {
    FontAwesomeIcon},
  data () {
    return {
        url : 'http://192.168.43.123:7878',
        loading:'false',
        write:'false',
        token : '',
        article : '',
        articleid : '',
        suggestionvalue:'',
        sentences:'',
        readability:[],
        summary : [],
        valence : [],
        wld : [],
        mark: [],
        errors : []
    }
  },
     watch:{
       myProp1 : function(value){
       this.article =value;
       console.log('insiderestcall')
       this.emotiontoken()
      },
      myProp2:function(value){
       this.markupsentence(value)
      },
      myProp3:function(value){
       
       this.getsuggestion(value)
      },
       suggestionvalue:function(value){
       alert('in')
       this.$emit('abc',value)
      },
      token:function(){
      alert(this.token);
       this.emotionarticleid()
      },
      articleid:function(value){
       alert(this.articleid);
       this.emotionvalence()
      },
      valence:function(value){
       alert(this.valence);
       this.$emit('listentovalencevalue',value)
       this.emotionsummary()
       
      },
      summary:function(plotvalue){
       this.emotionreadability()
       this.$emit('listentographvalue',plotvalue) 

      },
      readability:function(value){
        
       
      },
      mark:function(value){
      this.$emit('listentomarkvalue',value) 
      }
     
     },
    methods:{
     trail(){

     },
     getsuggestion(value){
       axios.post(`http://www.mocky.io/v2/5a8dac8c2f000052044f2479`,{headers: { 
     'Access-Control-Allow-Origin' : '*',
      'Access-Control-Allow-Headers' : 'Authorization',
      'X-Requested-With': 'XMLHttpRequest'
      
      }}).then(response => { console.log(response.data.ID)
         this.suggestionvalue= response.data;
         
         
      })
      //mark error
      .catch(e => {
      this.errors.push(e);
      console.log(this.errors);
       });
     },
     markupsentece(pos){
       axios.post(this.url + `/emotionBreakup `,{'ID' : this.articleid},{headers: { 'Authorization' : 'Bearer ' + this.token,
     'Access-Control-Allow-Origin' : '*',
      'Access-Control-Allow-Headers' : 'Authorization',
      'X-Requested-With': 'XMLHttpRequest'
      
      }}).then(response => { console.log(response.data.ID)
         this.sentences = response.data;
         alert(this.sentences)
         console.log(this.sentences[pos].sentence)
         this.mark= this.sentences[pos].sentence
         console.log('before')
         console.log(this.mark)
         
      })
      //mark error
      .catch(e => {
      this.errors.push(e);
      console.log(this.errors);
       });
       
    },
     emotiontoken(){
     this.loading = 'true';
     axios.get( this.url + `/getToken `,{crossdomain:true }).then(response => {
        this.token = response.data
         console.log(this.token);
       })
      // gettoken error  
      .catch(e => {
      this.errors.push(e)
      });
      },
      emotionarticleid(){
          axios.post( this.url + `/article `,this.article,{headers: { 'Authorization' : 'Bearer ' + this.token,
     'Access-Control-Allow-Origin' : '*',
      'Access-Control-Allow-Headers' : 'Authorization',
      'X-Requested-With': 'XMLHttpRequest'
      
      }}).then(response => { console.log(response.data.ID)
         this.articleid= response.data.ID;
         console.log(this.articleid);
         console.log(this.token);

      })
      //articleid error
      .catch(e => {
      this.errors.push(e);
      console.log(this.errors);
       });
      },
      emotionvalence(){
      //article valence

      axios.post(this.url + `/articleValence`,{'ID' : this.articleid},{headers: { 'Authorization' : 'Bearer ' + this.token,
     'Access-Control-Allow-Origin' : '*',
      'Access-Control-Allow-Headers' : 'Authorization',
      'X-Requested-With': 'XMLHttpRequest'
      
      }}).then(response => { console.log(response.data)
         this.valence= response.data;
         alert(this.valence);

    })
    // article valence error
    .catch(e => {
      this.errors.push(e);
      console.log(this.errors);
    });
     },
     emotionsummary(){
    //article summary

           axios.post(this.url + `/articleSummary`,{'ID' : this.articleid},{headers: { 'Authorization' : 'Bearer ' + this.token,
     'Access-Control-Allow-Origin' : '*',
      'Access-Control-Allow-Headers' : 'Authorization',
      'X-Requested-With': 'XMLHttpRequest'
      
      }}).then(response => { console.log(response.data)
         this.summary= response.data;
         console.log(this.sumary);
       

    })
    // article summary error
    .catch(e => {
      this.errors.push(e);
      console.log(this.errors);
    });
    },
     emotionreadability(){
  // article readability

       axios.post(this.url + `/articleReadability`,{'ID' : this.articleid},{headers: { 'Authorization' : 'Bearer ' + this.token,
     'Access-Control-Allow-Origin' : '*',
      'Access-Control-Allow-Headers' : 'Authorization',
      'X-Requested-With': 'XMLHttpRequest'
      
      }}).then(response => { console.log(response.data)
         this.readability= response.data;
         this.loading='false'
         this.write='true'
          console.log(this.readability);
      

    })
     // article readability error
    .catch(e => {
      this.errors.push(e);
      console.log(this.errors);
    });
    }

    } 
    
  }   


</script>
 

 <style>
  
  #load {
    background-color:DodgerBlue;
    width:100px;
    margin-left:150px;
    margin-top: 100px;
  }

  #emotionbuttons {
   margin-top:50px;
   margin-bottom:-30px;
   margin-left:80px;
  }

  #formality {
   margin-bottom:-50px;
  }

  #data{
   color:#fff;
  }

     
.loading-icon {
  position: relative;
  width: 20px;
  height: 20px; 
  margin:50px auto;
}

.loading-icon:before {
  content: "\f110";
  font-family: FontAwesome;
  font-size:20px;
  position: absolute;
  top: 0; 
}

 </style>