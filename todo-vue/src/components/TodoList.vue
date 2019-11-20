<template>
  <div class="todo-list">
    <ul>
      <li @click="todo_click(todo)" v-for="todo in todos" :key="todo.id">
        <input @change="updatedTodo(todo)" type="checkbox" v-model="todo.is_completed">
        {{todo.title}}
        <button @click="todo_delete(todo)">X</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    name:'TodoList',
    props :{
      todos: {
        type : Array,
        required : true
      }
    },
    methods : {
      todo_click(todo) {
        todo.is_completed = !todo.is_completed
      },
      todo_delete(todo) {
        this.$session.start()
        const token = this.$session.get('jwt')
        const options = {
          headers : {
          Authorization : `JWT ${token}`
          }
        }
        axios.delete(`http://127.0.0.1:8000/api/v1/todos/${todo.id}/`,options)
        .then(response => {
          console.log(response)
          this.todos = this.todos.filter( el => {
            return el !== todo
          })
        })
        .catch(error => {
          console.log(error)
        })
      },
      updatedTodo(todo) {
        console.log(todo)
        this.$session.start()
        const token = this.$session.get('jwt')
        const options = {
          headers : {
          Authorization : `JWT ${token}`
          }
        }
        // const data = {
        //   "id" : todo.id,
        //   "title" : todo.title,
        //   "user" : todo.user,
        //   "is_completed" : todo.is_completed
        // }
        axios.put(`http://127.0.0.1:8000/api/v1/todos/${todo.id}/`,todo,options)
        .then(response => {
          console.log(response)
        })
      }
    }
}
</script>

<style>

</style>