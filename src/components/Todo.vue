<template>
  <div class="container">
    
    <div v-bind:class="this.showInput ? 'showInput' : null " class="menu-container">
      <div class="display">
      {{ `${this.todo.filter(t => t.done === true).length}/${this.todo.length}` }} Tasks
      <i v-on:click="showInputToggle" class="fas fa-plus add-btn"></i>
    </div>
    <div class="menu">
      <i v-on:click="filters('all')" class="fas fa-list-ul" v-bind:class="this.active === 'all' ? 'active' : null" ></i>
      <i v-on:click="filters('pending')" class="far fa-square" v-bind:class="this.active === 'pending' ? 'active' : null" ></i>
      <i v-on:click="filters('done')" class="far fa-check-square" v-bind:class="this.active === 'done' ? 'active' : null" ></i>    
    </div>

    </div>
    <div class="task-container">
      <div class="row" v-for="(task, index) in filterList" v-bind:key="index" >
      <input v-on:click="doneTask(index)" type="checkbox" v-bind:checked="task.done" />
      <p v-bind:class="task.done ? 'done' : null " >{{ task.text }}</p>
      <i v-on:click="deleteTask(index)" class="far fa-trash-alt"></i>
    </div>
    </div>

    <div class="addTask">
      <input v-model="newTask" type="text" />
      <button v-on:click="addTask" >Add</button>
    </div>
    
  </div>
</template>

<script>
export default {
  name: 'Todo',
  data(){
    return {
      todo: [{ text: 'Task number 1', done: false }, { text: 'Task number 2', done: true }, { text: 'Task number 3', done: false }],
      filterList: [{ text: 'Task number 1', done: false }, { text: 'Task number 2', done: true }, { text: 'Task number 3', done: false }],
      active: 'all',
      showInput: false,
      newTask: ''
    }
  },
  methods: {

    addTask(){
      if(this.newTask !== ''){
        this.todo.push({ text: this.newTask, done: false })
        this.filters(this.active)
        this.newTask = ''
      }
    },

    showInputToggle(){
      this.showInput = !this.showInput
    },

    doneTask(i){
      this.todo[i].done = !this.todo[i].done
    },

    deleteTask(i){
      this.todo.splice(i, 1)
      this.filters(this.active)
    },

    filters(filter){
      this.active = filter
      if(filter === 'all'){
        this.filterList = this.todo
      }
      if(filter === 'pending'){
        this.filterList = this.todo.filter(task => task.done === false)
      }
      if(filter === 'done'){
        this.filterList = this.todo.filter(task => task.done === true)
      }

    }

  }
}
</script>

<style scoped>

.menu-container{
  position: absolute;
  top: 20vh;
  left: 50%;
  width: 400px;
  transform: translateX(-50%);
  z-index: 2;
  transition: all 0.3s ease-in-out;
}
.menu-container.showInput{
  transform: translate(-50%, -61px);
}

.task-container{
  position: absolute;
  top: calc(20vh + 154px);
  left: 50%;
  width: 400px;
  transform: translateX(-50%); 
  background: white;
}

.display{
  background: #7888B9;
  height: 100px;
  font-size: 2em;
  color: white;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  padding: 0px 24px;
  border-top-left-radius: 8px;
  border-top-right-radius: 8px;
}

.display .add-btn{
  position: absolute;
  height: 50px;
  width: 50px;
  border-radius: 50%;
  background: linear-gradient(to top right, #EE7AA7, #E5BECA);
  bottom: -25px;
  right: 10%;
  font-size: 0.5em;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
  cursor: pointer;
}

.menu{
  background-color: white;
  border-top: 1px solid rgba(0, 0, 0, 0.137);
  border-bottom: 1px solid rgba(0, 0, 0, 0.164);
  text-align: left;
  padding: 0px 12px;
}

.menu i{
  padding: 18px 0px;
  width: 50px;
  position: relative;
  text-align: center;
}


.menu i.active::after{
  content: '';
  position: absolute;
  bottom: -1px;
  height: 2px;
  left: 0px;
  width: 100%;
  background: purple;
}

.row:nth-child(1){
    border-top: 1px solid rgba(0, 0, 0, 0.164);
}
.row{
  border-bottom: 1px solid rgba(0, 0, 0, 0.164);
  padding: 18px;
  text-align: left;
  display: flex;
  align-items: center;
  animation: showTask 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  transform-origin: top;
}

.row input{
  margin-right: 12px;
}

.row p{
  margin: 0px;
  flex: 1;
}
.row p.done{
  color: rgba(0, 0, 0, 0.247);
  text-decoration: line-through;
}

.row i{
  transition: all 0.2s ease-in-out;
}
.row i:hover{
  color: rgb(129, 12, 12);
  cursor: pointer;
}

@keyframes showTask {
  0%{
    transform: translateY(-100%);
  }
  100%{
    transform: translateY(0);
  }
}




.addTask{
  position: absolute;
  top: calc(20vh + 154px - 61px);
  left: 50%;
  width: 400px;
  padding: 12px;
  background: white;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  z-index: 1;
}

.addTask input{
  flex: 1;
  padding: 9px 8px;
  border-radius: 4px;
  border: 1px solid rgba(0, 0, 0, 0.219);
  font-size: 0.9em;
  border-top-right-radius: 0px;
  border-bottom-right-radius: 0px;
  border-right: none;
}

.addTask button{
  padding: 10px 24px;
  font-size: 0.9em;
  border: none;
  border-radius: 4px;
  border-top-left-radius: 0px;
  border-bottom-left-radius: 0px;
  background: linear-gradient(to top right, #EE7AA7, #E5BECA);
  color: white;
  font-weight: 600;
  cursor: pointer;
}

</style>
