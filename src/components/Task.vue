<template>

  <div class="hello">
      
    
        <b-row class="text-center">
        <b-col v-if = task.state><b-icon  icon="check-square" scale="2" variant="success"></b-icon></b-col >
      
        <b-col v-else ><b-icon icon="x-circle" scale="2" variant="danger"></b-icon></b-col >
          
        <b-col lg="5"><p>Tache: {{ task.title }}</p></b-col >
         
        <b-col lg="5"><p>Responsable: {{ task.responsible }}</p></b-col>
        </b-row>
        <b-row>
        
        <b-col><b-icon icon="arrow-repeat" scale="2" variant="primary" v-on:click="toglih()"></b-icon></b-col>
        
        <b-col><b-icon icon="trash" scale="2" variant="danger" v-on:click="supTask(task.id)"></b-icon></b-col>
        </b-row>
    <form v-if="toggle">
        <br>
        <b-row>
        <b-input id="taskid" v-bind:value = task.id hidden/>
        <b-p>titre : <b-input id="title" v-bind:value = task.title /></b-p>
        <b-p>responsable : <b-input id="responsible" v-bind:value = task.responsible /></b-p>
        <b-p>Terminée <input id="state" type="checkbox" v-bind:checked= task.state ></b-p>
        </b-row>
        
        <br>
        <b-button v-on:click="update()"><b-icon icon="eject" scale="1"></b-icon>Valider</b-button>
      
    </form>
    
    
    <br>
  </div>
 
  
</template>
<script>
import axios from 'axios';
export default {
  name: 'Task',
  props: ["task"],
   data(){
   return {
     toggle:false,
     taskid:String,
     title:String,
     responsible:String,
     state:Boolean
   }
  },
  methods:{
    toglih(){
      if(this.toggle)
      this.toggle=false;
      else
      this.toggle=true;
    },
    update(){
       if( document.getElementById("title").value && document.getElementById("responsible").value && isNaN(document.getElementById("title").value) && isNaN(document.getElementById("responsible").value))
        {axios.put("http://localhost:8000/uptask",{
        id:document.getElementById("taskid").value,
        title:document.getElementById("title").value,
        responsible:document.getElementById("responsible").value,
        state:document.getElementById("state").checked
        })
      .then(()=>{alert("Mise a jour effectué avec succes");window.location.reload()})
      .catch(()=>{alert("verfier les champs")})}
      else
      alert("le titre ou le responsable est invalide");
       
    },
    supTask(id){
           axios.delete(`http://localhost:8000/removetask/${id}`).then(()=>{alert("tache supprimée avec succes");window.location.reload()}).catch(()=>alert(""));
    }

  },
  created(){
    this.toggle=false;
    
  }
   
  
}
</script>