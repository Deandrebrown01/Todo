<script setup>
import { ref, watch } from 'vue';
let todos = ref(JSON.parse(window.localStorage.getItem('todos'))??[])
let newTodo = ref('')
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
</script>

<template>
<h1>To Do List (Do It Now)</h1>
<ul>
<li v-for="(todo,index ) in todos" :class= "{completed: todo.complete}"> 
<input type="checkbox" v-model="todo.complete">
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
 
</style>
