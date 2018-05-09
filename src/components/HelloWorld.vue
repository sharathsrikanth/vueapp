<template>
   <div id="app">
     <vue-editor v-model="content"></vue-editor>
   </div>
 </template>
 
 <script>
   import { VueEditor } from 'vue2-quill-editor'
 
   export default {
 
   components: {
      VueEditor
   },
 
   data() {
       return {
         content: '<h1>Some initial content</h1>'  
       }
     }
   }
 </script> 

 <style scoped>
   #app {
    width:500px;
    heigth:300px;
    margin-top:50px;
    margin-left:20px;
   }
 </style>