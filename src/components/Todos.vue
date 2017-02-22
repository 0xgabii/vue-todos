<template>
  <div class="wrapper">
    <h1>{{ welcome }}</h1>
    <div class="todos-box">
      <div class="todos-input-box">        
        <input 
          type="checkbox"
          id="toggleAll"           
          v-model="completeAll">
        <label for="toggleAll"></label>
        <input 
          class="todos-input"
          placeholder="create todo..."
          v-model="newTodo"
          @keyup.enter="addTodo">  
      </div>
      <ul class="todos">
        <transition-group name="fade">
          <li :key="index" v-for="(todo, index) in filterdTodos">
            <input 
              type="checkbox" 
              :id="'chkbox' + index"
              v-model="todo.complete">
            <label :for="'chkbox' + index"></label>
            <span 
              :contenteditable="true" 
              @blur="editTodo(todo, $event)">{{todo.text}}</span>
            <button @click="deleteTodo(todo)">✖</button>
          </li>
        </transition-group>
      </ul> 
      <transition name="fade">
        <div class="todos-control"  v-show="todos.length">
          <span>{{ remaining }} items left</span>
          <ul>
            <li v-for="filter in filters"
              :class="{active: filter === filterType}" 
              @click="filterChange(filter)">{{ filter }}</li>
          </ul>
          <button
            :style="{opacity: todos.length > remaining ? 1 : 0}"
            @click="removeCompletedTodo">Clear completed</button>
        </div>
      </transition>
    </div>         
  </div>
</template>

<script>
const filters = {
  all: (todos) => {
    return todos;
  },
  active: (todos) => {
    return todos.filter(data => !data.complete);
  },
  completed: (todos) => {
    return todos.filter((data) => data.complete);
  },
}

export default {
  name: 'todos',
  data() {
    return {
      welcome: 'To-do list',
      newTodo: '',
      todos: [
        {
          text: 'Practice Vue.js',
          complete: false
        }
      ],
      filters: [
        'all',
        'active',
        'completed'
      ],
      filterType: 'all'
    }
  },  
  computed:{
    filterdTodos() {
      return filters[this.filterType](this.todos);
    },
    remaining() {
      return filters.active(this.todos).length;
    },
    completeAll: {
      get(){
        return this.remaining === 0; 
      },
      set(value){
        this.todos.forEach(data => { 
          data.complete = value
        });
      }
    }
  },
  methods:{
    addTodo() {
      if(!this.newTodo.trim()) return;
      this.todos.push({
        text: this.newTodo,
        complete: false
      });
      this.newTodo = '';
    },
    editTodo(todo, e) {
      this.todos[this.todos.indexOf(todo)].text = e.target.textContent;      
    },
    deleteTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
    },
    removeCompletedTodo() {
      this.todos = filters.active(this.todos);
    },
    filterChange(filter) {
      this.filterType = filter;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  /* transition-group css */
  .fade-enter-active,
  .fade-leave-active {
    transition: all 0.3s
  }
  .fade-enter,
  .fade-leave-to{    
    opacity: 0;
  }
  .fade-enter{
    transform: translateX(-1rem);
  }
  .fade-leave-to {
    transform: translateX(1rem);    
  }
  /* Component css */
  .wrapper{
    width: 35rem;  
    margin: 12rem auto;
  }
  h1{    
    margin-bottom: 2rem;
    font-size: 3rem;
    text-align: center;
  }    
  .todos-box{    
    color: #585858;
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
  }
  .todos-input-box{
    position: relative;    
    background-color: #fff;
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2);
  }
  input[type="checkbox"]{
    position: absolute;
    width: 0;
    height: 0;
    opacity: 0;
  }
  input[type="checkbox"]:checked ~ label{
    border-color: gray;
  }
  input[type="checkbox"]:checked ~ label:after{
    color: gray;
    opacity: 1;
    transform: scale(1);
  }
  label{
    position: absolute;
    left: 1rem;
    top: 50%;        
    width: 1.5rem;
    height: 1.5rem;
    border: 1px solid lightgray;
    border-radius: 100%;
    cursor:pointer;
    transform: translateY(-50%);
    transition: all 0.2s;
  }
  label:after{
    content: '✔';  
    position: absolute;
    left: 20%;
    font-size: 1rem;
    color: lightgray;
    opacity: 0;
    transform: scale(0.5);
    transition: all 0.2s;
  }
  label:hover, 
  label:hover:after {
    border-color: gray;
    color: gray;
  }
  .todos-input{
    padding: 0.9rem 0.9rem 0.9rem 3.3rem;
    width: 100%;
    background-color: transparent;
    border: none;
    outline: none;
    font-size: 1.8rem;
    transition: all 0.2s;
  }
  .todos{
    list-style: none;
  }
  .todos li{
    position: relative;
    padding: 0.9rem 0.9rem 0.9rem 3.3rem;
    border-bottom: 1px solid lightgray;
    font-size: 1.5rem;
  }
  .todos li > span{    
    position: relative;
    transition: all 0.2s;
  }
  .todos li > span:after{
    content: '';
    display: block;
    margin: auto;
    position: absolute;    
    left: 0;
    top: 50%;    
    width: 0px;
    height: 2px;
    background-color: transparent;   
    transition: all 0.5s;
  }
  .todos li > input[type="checkbox"]:checked ~ span{
    color: lightgray;
  }
  .todos li > input[type="checkbox"]:checked ~ span:after{
    width: 100%;
    background-color: lightgray;
  }
  .todos li > button{
    position: absolute;
    right: 0;
    top: 3%;
    padding: 0.9rem 1.5rem;        
    background-color: transparent;
    border:none;    
    outline: none;
    font-size: 1.25rem;    
    color: gray;
    opacity: 0;  
    cursor: pointer;
    transition: all 0.2s;      
  }
  .todos li:hover > button{
    opacity: 1;
  }
  .todos-control{
    display: flex;
    justify-content: space-between;
    padding: 0.9rem;
  }
  .todos-control > ul{
    list-style: none;
  }
  .todos-control > ul > li{
    display: inline-block;
    padding: 0 0.5rem;
    color: gray;
    text-transform: capitalize;
    cursor: pointer;
    transition: all 0.3s;
  }
  .todos-control > ul > li:hover,
  .todos-control > ul > li.active{
    color: black;
  }
  .todos-control > button{
    position: relative;
    background-color: transparent;
    border: none;
    outline: none;
    font-size: 1rem;
    color: gray;
    transition: all 0.3s;
  }
  .todos-control > button:hover{
    text-decoration: underline;
  }
</style>
