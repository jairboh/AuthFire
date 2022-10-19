<template>
  <div class="container">
    <h2 class="text-center mt-5">To do list :)</h2>
    <br>
    <link rel="stylesheet" href="styles.css">
    
    <!-- Entrada de datos-->
    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Escribe una nueva tarea" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">Enviar</button>
    </div>

    <!--Tabla de tareas-->
    <table class="table table-bordered mt-5" id="tablalista">
  <thead>
    <tr>
      <th scope="col">Tareas</th>
      <th scope="col">Estatus</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task,index) in tasks" :key="index">
      <td>
        <span :class="{'finished':task.status==='Terminada'}">{{task.name}}</span>
      </td>
      <td style="width: 120px">
        <span @click="changeStatus(index)" class="pointer"
        :class="{'text-danger':task.status==='Pendiente',
        'text-warning':task.status==='En proceso',
        'text-success':task.status==='Terminada'
      }"
        >
        {{firstCharUpper(task.status)}}

          </span>
        </td>
      <td>
        <div class="text-center" @click="editTask(index)">
          <span class="fa fa-pen"></span>
        </div>
      </td>
      <td>
        <div class="text-center" @click="deleteTask(index)">
          <span class="fa fa-trash"></span>
        </div>
      </td>
    </tr>
  </tbody>
</table>
  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  data(){
    return{
      
    }
  },
  data(){
    return{
      task:'',
      editedTask: null,
      availableStatuses:['Pendiente', 'En proceso','Terminada'],
       tasks:[
        {
          name: 'Hacer tarea programacion',
          status: 'Pendiente',
        },
       ]
    }
  },

  methods: {
    submitTask(){
      if(this.task.length === 0)return;

      if(this.editedTask===null){
        this.tasks.push({
        name: this.task,
        status: 'Pendiente'
        });
      }else{
        this.tasks[this.editedTask].name=this.task;
        this.editedTask=null;
      }

      this.task = '';
    },
    
    deleteTask(index){
      this.tasks.splice(index,1);
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask=index;
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex>2)newIndex=0;
      this.tasks[index].status=this.availableStatuses[newIndex];
    },

    firstCharUpper(str){
      return str.charAt(0).toUpperCase()+str.slice(1);
    }
  }
};
</script>

<style scoped>
.pointer{
  cursor: pointer;
}

.finished{
  text-decoration: line-through;
}
</style>