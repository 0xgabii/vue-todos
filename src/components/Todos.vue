<template>
  <div class="wrapper">
    <h1>{{ msg }}</h1>
    <div class="todos-box">
      <div class="todos-input-box">        
        <input id="toggleAll" type="checkbox">
        <label for="toggleAll"></label>
        <input 
          class="todos-input"
          placeholder="create todo..."
          v-model="newTodo"
          @keyup.enter="addTodo">  
      </div>
      <ul class="todos" v-show="todos.length > 0">
        <li v-for="(todo, index) in todos">
          <input :id="'chkbox' + index" type="checkbox">
          <label :for="'chkbox' + index"></label>
          <span>{{todo.text}}</span>
        </li>
      </ul>
      <div class="todos-state" v-show="todos.length > 0">            
        {{ todos.length }} items left
      </div>
    </div>            
  </div>
</template>

<script>
export default {
  name: 'todos',
  data() {
    return {
      msg: 'To-do list',
      newTodo: '',
      todos: [
        {text: 'Practice Vue.js'}
      ]
    }
  },  
  methods:{
    addTodo() {
      if(!this.newTodo.trim()) return;
      this.todos.push({
        text: this.newTodo
      });
      this.newTodo = '';
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .wrapper{
    width: 30rem;  
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
    background-color: rgba(0,0,0,0.01);
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
  .todos-input:focus{
    background-color: #fff;
  }
  .todos{
    list-style: none;
  }
  .todos > li{
    position: relative;
    padding: 0.9rem 0.9rem 0.9rem 3.3rem;
    border-bottom: 1px solid lightgray;
    font-size: 1.5rem;
  }
  .todos > li > span{    
    position: relative;
    transition: all 0.2s;
  }
  .todos > li > span:after{
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
  .todos > li > input[type="checkbox"]:checked ~ span{
    color: lightgray;
  }
  .todos > li > input[type="checkbox"]:checked ~ span:after{
    width: 100%;
    background-color: lightgray;
  }
  .todos-state{
    padding: 0.9rem;
  }
</style>
