<template>
 
    <b-container>
      <b-button v-b-toggle.collapse-1 variant="success">Ajouter Une Todoliste</b-button>
  <b-collapse id="collapse-1" class="mt-2">
    <b-card>

      <b-row>
       
       <b-input v-model="text"/>
      
         <br>
       <b-button v-on:click="addlist">Ajouter</b-button>
          
       </b-row>
      
    </b-card>
  </b-collapse>
     
    <br>
    <b-row>
    <b-col lg="4" v-bind:key="l.id" v-for="l in todos">
      
      <HelloWorld v-bind:list="l"/>
      
    </b-col>
    </b-row>
  
  </b-container>
</template>

<script>

// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import axios from "axios";
export default {
  name: 'Home',
  components: {
    HelloWorld
  },
  data(){
    return {
      todos:[],
      text:'',
    }
  },
  methods:{
      addlist(){
        if(this.text && isNaN(this.text))
        {
          axios.post("http://localhost:8000/addtodolist",{name:this.text})
          .then(()=> {alert("todoliste est ajoutée");window.location.reload()})
          .catch(()=> {alert("erreur");window.location.reload()});
        }
        else
        alert("Vérifiez le nom..");
      }
  },
  created()
  {
    axios.get("http://localhost:8000/gettodolist")
    .then(res =>{this.todos=res.data;})
    .catch(err=>console.log(err));
    
  }
 
}
</script>
