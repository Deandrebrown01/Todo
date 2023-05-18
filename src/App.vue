<script setup>
import { ref, watch } from 'vue';
let todos = ref(JSON.parse(window.localStorage.getItem('todos'))??[])
let newTodo = ref('')
let filter = ref('all')
function number () {
todos.value.push({
    text: newTodo.value,
    complete: false
})

newTodo.value = ''
}
function deleteTodo(index){
todos.value.splice(index, 1)

}

watch (todos, function(value){
    window.localStorage.setItem('todos',JSON.stringify(value))
},{deep:true})

function todoFilter(todo){
    if (filter.value== 'active'){
     return todo.complete == false
    } else if (filter.value=='completed') {
        return todo.complete == true
    } else{
      return true
    } 
}

function activeFilter(todo){
  return todo.complete == false
}
</script>

<template>
<h1>To Do List (Do It Now)</h1>
<p v-if="todos.length > 0">
<input name="filter" type="radio" value="all" v-model="filter">
<label>All</label>
<input name="filter" type="radio" value="active" v-model="filter">
<label>Active</label>
<input name="filter" type="radio" value="completed" v-model="filter">
<label>Complete</label>
</p>
<p>{{ todos.filter(activeFilter).length }} items left</p>
<ul>
<li v-for="(todo,index ) in todos.filter(todoFilter)" :class= "{completed: todo.complete}"> 
<label class="container">
<input type="checkbox" checked="checked" v-model="todo.complete">
<span class="checkmark"></span>
</label>
<span :class="{completed: todo.complete}">{{ todo.text }}</span>
<button @click="deleteTodo(index)">delete</button>
{{ todo.text }}
</li>
</ul>
<input v-model="newTodo" @keydown.enter="number">
<button @click="number">Add Todo</button>



</template>

<style>
 body{
font-family: 'Times New Roman', Times, serif;
background-color: rgb(171, 207, 233);
text-align: center;
 }
 .completed{
    text-decoration: line-through;
    color: rgb(0, 0, 0);
 }
 
.container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}


.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}


.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
}


.container:hover input ~ .checkmark {
  background-color: #ccc;
}


.container input:checked ~ .checkmark {
  background-color: #003f72;
}


.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}


.container input:checked ~ .checkmark:after {
  display: block;
}


.container .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}
</style>
