<template >
<div class="quillWrapper" >
  <div id="edit" >
  <div ref="quillContainer" :id="id" v-on:click="test" ></div> 

  </div>
  <div >
  <button id="analyze" v-on:click="saveContent" type="button"  class="btn btn-primary">Analyze</button>
  </div>
</div>
</template>

<script>
import Quill from 'quill'
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import restcalls from './restcalls'
import temp from './temp'


var defaultToolbar = [
  ['bold', 'italic', 'underline', 'strike'],
  ['blockquote', 'code-block', 'image'],
  [{ 'list': 'ordered'}, { 'list': 'bullet' }],
  [{ 'indent': '-1'}, { 'indent': '+1' }],
  [{ 'header': [1, 2, 3, 4, 5, 6, false] }],
  [{ 'color': [] }, { 'background': [] }],
  [{ 'font': [] }],
  [{ 'align': [] }],
  ['clean']
]
export default {
  name: 'vue-editor',
  props: {
    mark1:{ type : Object},
    value: String,
    id: {
      type: String,
      default: 'quill-container'
    },
    placeholder: String,
    disabled: Boolean,
    editorToolbar: Array
  },
  data() {
    return {
      uiu:'',
      quill: null,
      editor: null,
      toolbar: this.editorToolbar ? this.editorToolbar : defaultToolbar,
    }
  },
  mounted() {
    this.initializeVue2Editor()
    this.handleUpdatedEditor()
  },
  watch: {
    value (val) {
      if (val !=  this.editor.innerHTML && !this.quill.hasFocus()) {
        this.editor.innerHTML = val
      }
    },
    disabled(status) {
      this.quill.enable(!status);
    },
    mark1:function(emotion){
       //  var str1 = "`";
        // var str3 = "`";
        // var str2 = emotion;
        // var res = str1.concat(str2);
        // var res2 = res.concat(str3);
          
          //var myobj = JSON.parse(res2);
        // alert(myobj);
          

          var emotionsentences = emotion;
          
          
          
        //  var emotionwords = emotion.words;
          var stringlength = [];
         // var wordlength = [];
    
        emotionsentences.forEach(function(str) { 
            stringlength.push([str.length]);
        });
        
        /*calculating the length of the emotion words
         emotionwords.forEach(function(str) { 
            wordlength.push([str.length]);
        }); */
     
     

    function getIndicesOf(searchStr, str, caseSensitive) {
        var searchStrLen = searchStr.length;
        if (searchStrLen == 0) {
            return [];
        }
        var startIndex = 0, index;
        if (!caseSensitive) {
            str = str.toLowerCase();
            searchStr = searchStr.toLowerCase();
        }
        if((index = str.indexOf(searchStr, startIndex)) > -1) {
           return index;
        }
       
        } 
      var sentenceindices = [];
        var content= this.editor.innerHTML.replace(/(<([^>]+)>)/ig,"")
       this.quill.setText(content);
       
     for(var i=0; i < emotionsentences.length ; i++) {
       sentenceindices[i] = getIndicesOf(emotionsentences[i], content);
       this.quill.formatText(sentenceindices[i],Number(stringlength[i]), 'background', '#C3E4ED'); 
       } 

   /*   var wordindices = [];
      
     for(var i=0; i < emotionwords.length ; i++) {
       wordindices[i] = getIndicesOf(emotionwords[i], content);
       this.quill.formatText((wordindices[i])-1,(Number(wordlength[i]))+2, 'background', '#FA8072');
         
       } */
    }
  },
  methods: {
     test(){
      
      var s = window.getSelection();
          s.modify('extend','backward','word');        
          var b = s.toString();

          s.modify('extend','forward','word');
          var a = s.toString();
          s.modify('move','forward','character');
            var c=b+a
          
          this.$emit('suggestion',c);
          
     },
     highlight() { alert('sjcnsj')
      
    },
   initializeVue2Editor() {
      this.setQuillElement()
      this.setEditorElement()
      this.checkForInitialContent()
    },
    setQuillElement() {
      this.quill = new Quill(this.$refs.quillContainer, {
        modules: {
          toolbar: this.toolbar
        },
        placeholder: 'Type something ...',
        theme: 'snow',
        readOnly: this.disabled ? this.disabled : false,
      })
      
    },
    setEditorElement() {
      this.editor = document.querySelector(`#${this.id} .ql-editor`)
    },
    checkForInitialContent() {
      this.editor.innerHTML = this.value || ''
    },
    saveContent() {
       var strip = this.editor.innerHTML.replace(/(<([^>]+)>)/ig,"");
       var data = {'article' : strip}
     //  restcalls.methods.emotionvalues(data);
       
        console.log("inside");
       this.$emit('listenchange',data);
    },
    handleUpdatedEditor() {
      this.quill.on('text-change', () => {
        this.$emit('input', this.editor.innerHTML)
      })
    }
  }
}
</script>

<style scoped>

  

  #edit {
    background-color:#E7E1CB;
    width:500px;
    margin-top:100px;
    margin-left:20px;
    position:relative;
  }
  #quill-container{
    background-color:#E7E1CB;
    height:400px;
  }
   #analyze {

   margin-left:20px;
   margin-top:10px;
   } 
</style>