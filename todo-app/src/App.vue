<template>
<div class="container justify-content-center align-items-center d-flex mt-5">

  <div class="row">
    <div class="col md-8  mid">
      <label class="title-1 text-center"> Todo-App VUEJS </label>
      <form @submit.prevent="addNewTask">
        <div class="input-group ">
          <input type="text" class="form-control" placeholder="Görev ekle" v-model="newTask" >
          <div class="input-group-append">
            <button class="btn btn-success" type="submit">Ekle</button>
          </div>
        </div>
        </form>
      <hr>


            <ul class="list-group">
              <transition-group
                enter-active-class="animated rotateInDownRight"
                leave-active-class="animated pulse"
                model="out-in">
            
                  <li  v-for="(item, index) in tasks" :key="index" class="list-group-item mr-3 mb-1" :class="item.done ? 'bg-secondary' : 'bg-success' ">
                    {{item.title}}
                    <strong class="float-right "> 
                        <button class="btn btn-dark mr-2" type="button" @click="showDone(index)">
                          <i :class="item.done ? 'fas fa-toggle-off fas-md kırmızı' : 'fas fa-toggle-on size:5px yesil'"></i>
                        </button>
                        <button class="btn btn-danger" type="button" @click="deleteTask(index)" >
                          <i class="fa fa-trash fa-md"  > </i>
                          </button>    
                    </strong>
                  </li>
                  
              </transition-group>
            </ul>


    </div>
  </div>
  
        
          
</div>
</template>

<script>


export default {
  data() {
    return {
      newTask : "",
      tasks:[],
    }
  },
  methods:{
    addNewTask(){
      if (this.newTask == '') {
        alert('Boş görev giremessin!');
        return;
      }

        this.tasks.push({
          title : this.newTask,
          done : true
      });

      this.newTask = "";
      
    },
    deleteTask(index){
      this.tasks.splice(index , 1);
    },
    showDone(index){
        const item = this.tasks[index];

        item.done = !item.done;
    }
  },
  mounted() {
    const jsonLocal = localStorage.getItem('todoapp');
    const item = JSON.parse(jsonLocal ? jsonLocal : '[]');
    this.tasks =  item;
  },
  watch : {
    tasks: {   // objenin adı
      deep: true, // değişen tüm değerlerin izlenmesi için true
      handler(){  
        const item  = this.tasks;
        localStorage.setItem('todoapp',JSON.stringify(item)) // local değerde girilen objeyi json formatına çevirir.

      }
    }
  }
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body{
  background-color: #202124;
}

.mid{
  background-color: aquamarine;
  color: black;
  width: 500px;
  min-height: 600px;
}
.title-1{
  justify-content: center;
  align-content: center;
  display: flex;
  font-size: 30px;
  font: 700;
  color: gray;
  margin-bottom: 10px ;
}
li{
  list-style-type: none;
}
.kırmızı{
  color: brown;
}
.yesil{
  color: green;
}

.list-enter{

}
.list-enter-active{
  animation: list-in 1s ease-in-out forwards;
}
.list-leave{

}
.list-leave-active{
  animation: list-out 1s ease-in-out forwards;
}

@keyframes list-in{
  form{
    transform: translate(-50px);
  }
  to{
    transform: translate(0px);
  }
}
@keyframes list-out{
  form{
    transform: translate(0px);
  }
  to{
    transform: translate(-50px);
  }
}
</style>
