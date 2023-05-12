<script setup>
import { ref } from 'vue';
let todos = ref([])
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
</script>

<template>
<h1>My Todo Application</h1>
<ul>
<li v-for="(todo,index ) in todos"> 
<input type="Checkbox" v v-model="todo.complete">
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
 }
</style>
