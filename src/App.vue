<template>
  <div id="app">
    <h2 class="bg-success text-white text-center p-2">Lista de tareas de {{nombre}}</h2>

    <div class="container-fluid p-4">
      <div class="row py-2 mt2 ">
          <dov class="col text-center">
            <input type="checkbox" v-model="esconderCompletadas" class="form-check-input">
            <label class="form-check-label font-weight-bold">
              Esconder tareas completadas
            </label>
          </dov>
      </div>

      <div class="row text-center" v-if="filtroCompletadas.length == 0">
        <h3>No hay tareas por realizar! Excelente!!</h3>
      </div>
      <template v-else>
          
            <div class="row">
              <div class="col fw-bold">Tarea</div>
              <div class="col-2 fw-bold">Hecho</div>

            </div>

              <div class="row" v-for="tarea in filtroCompletadas" v-bind:key="tarea.accion">
                  <div class="col">
                    {{tarea.accion}}
                  </div>
                  <div class="col-2">
                    <input type="checkbox" class="form-check-input" v-model="tarea.hecho" >
                    
                  </div>
            </div>
      </template>
    

        <div class="row py-2">
            <dov class="col">
              <input type="text" v-model="nuevaTareaTexto" class="form-control"> 
            </dov>
            <div class="col-2">
              <button class="btn btn-success" v-on:click="crearNuevaTarea">Agregar</button>
            </div>
        </div>

        <div class="col text-center">
          <button class="btn btn-sm btn-danger" v-on:click="borrarTareasHechas">
            Borrar Completadas
          </button>
        </div>
       
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      "nombre":"Maxi",
      "tareas":[],
      "esconderCompletadas": false,
      "nuevaTareaTexto":""
    }
  },
  computed:{
    filtroCompletadas(){
      return this.esconderCompletadas ?
        this.tareas.filter(item => !item.hecho) : this.tareas
    }
  },
  methods: {
    crearNuevaTarea(){
      this.tareas.push({
        accion:this.nuevaTareaTexto,
        hecho:false
      });
      
      this.nuevaTareaTexto="";
    },
    borrarTareasHechas(){
      this.tareas=this.tareas.filter(item => !item.hecho);
    }
  
  },
  created(){
    let data=localStorage.getItem("lista_tareas");
    if (data != null){
       this.tareas=JSON.parse(data);
    }
  },
  updated(){
    localStorage.setItem("lista_tareas",JSON.stringify(this.tareas));
  }

}
</script>

<style>
#app {
  
}
</style>
