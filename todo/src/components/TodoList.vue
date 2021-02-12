<template>
  <div class="container">
    <h1 class="title">TODO LIST</h1>
    <br>
    <input type="text" placeholder="What needs to be done..." class="todo-input" v-model="newTodo" @keyup.enter="addTask()">
    <div class="todo-item" v-for="(todo, index) in todosFiltered" :key="todo.id">
      <div :class="{completed:todo.completed}"><input id="left-space" class="completed" type="checkbox" v-model="todo.completed">{{todo.title}}</div>
         <div class="remove-items" @click="deleteTask(index)">&times;</div>
    </div>
    <div class="container2">
      <div><label><input id="check-space" type="checkbox" :checked="!noRemaining" @change="checkAll()">Check All</label></div>
      <div>{{remaining}} task left</div>
    </div>
    <div class="container2">
      <div class="btnShape">
        <button :class="{ active: filter=='all' }" @click="filter='all'">All</button>&nbsp;&nbsp;
        <button :class="{ active: filter=='active' }" @click="filter='active'">Active</button>&nbsp;&nbsp;
        <button :class="{ active: filter=='completed' }" @click="filter='completed'">Completed</button>
      </div>
      <div class="btnShape">
        <transition name="fade">
        <button @click="clearCompleted" v-if="showClearBtn">Clear Completed Tasks</button>
        </transition>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data(){
    return{
      newTodo:'',
      todoId:3,
      filter:'all',
      todos:[
         {
        'id':1,
       'title':'Meeting',
       'completed':false
     },
       {
         'id':2,
         'title':'shopping',
         'completed':false
       },
        {
          'id':3,
          'title':'Groceries',
          'completed':false
        }
     ],
    }
  },
  computed:{
    remaining(){
      return this.todos.filter(todo => !todo.completed).length
    },
    noRemaining(){
      return this.remaining !== 0
    },
    todosFiltered() {
      if(this.filter=='all'){
        return this.todos
      }else if(this.filter=='active'){
        return this.todos.filter(todo=> !todo.completed)
      }else if(this.filter=='completed'){
        return this.todos.filter(todo=> todo.completed)
      }
      return this.todos
    },
    showClearBtn(){
     return this.todos.filter(todo => todo.completed).length>0
    }
  },
  methods:{
    addTask() {
      if (this.newTodo.length === 0) {
        return
      }
      this.todos.push({
        id: this.todoId,
        title: this.newTodo,
        completed: false
      })
      this.newTodo = ''
      this.todoId++
    },
    deleteTask(index){
      this.todos.splice(index,1)
    },
    checkAll(){
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    },
    clearCompleted(){
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  }
}
</script>

<style>
.container{
 background-color: yellow;
 padding: 10px;
}
.title{
  padding: 10px;
  font-family: fantasy;
}
.container2{
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top: 1px solid grey;
  margin-bottom: 14px;
  margin-top: 12px;
  padding: 10px;
  font-size: 16px;
  font-family: "Lucida Console";
}
.active{
  border-radius: 5px;
  background-color: green;
  color: white;
}
.todo-input{
  width: 100%;
  padding: 8px;
  font-size: 16px;
  margin-bottom: 20px;
  border-radius: 4px;
  background-color: ghostwhite;
  border: yellow;
  font-family: "Lucida Console";
}
.btnShape{
  margin-top: 10px;
}
#left-space, #check-space{
 margin: 10px;
}
.todo-item{
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 14px;
  margin-top: 12px;
  padding: 10px;
  font-size: 16px;
  font-family: "Lucida Console";
  }
.completed{
  text-decoration: line-through;
  color: deeppink;
}
.remove-items{
  cursor: pointer;
  font-size: 16px;
  margin: 10px;
}
.remove-items:hover{
  font-size: 20px;
  color: black;
  font-weight: bold;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .6s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>