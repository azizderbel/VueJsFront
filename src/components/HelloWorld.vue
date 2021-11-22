<template>
  
    <div>
        
      <h3>Todo list : {{list.name}}</h3>
            <br>
       <b-row>
         <b-col>
      <b-button v-on:click="toglih()" size="small" variant="outline-secondary">Ajouter une tache</b-button>
         </b-col>
         <b-col>
      <b-button v-on:click="sup(list.id)" size="small" variant="outline-danger">Supprimer la liste</b-button>
         </b-col>
       </b-row>
    <form v-if="toggle">
        <br>
        <b-row class="text-center">
        <b-col >
          <b-input placeholder="titre" id="title" v-model = title />
        </b-col>
        <b-col>
            <b-input placeholder="responsable" id="responsible" v-model = responsible />
        </b-col>
        <b-col>
          <b-button size="sm" v-on:click="Ajouter(list.id)">confirmer</b-button>
        </b-col>
        
        </b-row >
        
    </form>
   <br>
   <b-list-group-item v-bind:key="t.id" v-for="t in list.tasks">
           <Task v-bind:task="t"/>
           </b-list-group-item>
    <!--<div v-bind:key="t.id" v-for="t in list.tasks">
      <Task v-bind:task="t"/> 
   </div>-->
   
    </div>
  
 
  
</template>

<script>

import Task from '@/components/Task.vue'
import axios from 'axios'
export default {
  name: 'HelloWorld',
  components: {
    Task
  },
  props: ["list"],

  data(){
      return{
    toggle:false,
     title:'',
     responsible:''
     
           }
  },
 
  methods:{

    sup(id){
      axios.delete(`http://localhost:8000/removetodolist/${id}`)
      .then(()=>{alert("todolist est supprimée");window.location.reload()})
      .catch(()=>{alert("Videz la liste avant de supprimer");window.location.reload()});
    },
    toglih(){
      if(this.toggle)
      this.toggle=false;
      else
      this.toggle=true;
    },
    Ajouter(idt){
      if( this.title && this.responsible && isNaN(this.title) && isNaN(this.responsible) )
      {axios.post(`http://localhost:8000/addtask/${idt}`,{
        title:this.title,
        responsible:this.responsible,
        state:false
      }).then(()=>{alert("La tache est ajoutée avec succès");window.location.reload()}).catch(err=>console.log(err));
      }
      else
      alert("le titre ou le responsable est invalide");
    }

  },
  created(){
    this.toggle=false;
    
  }
  
   
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
