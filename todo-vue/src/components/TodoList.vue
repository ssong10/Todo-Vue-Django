<template>
  <div class="todo-list">
    <ul>
      <li @click="todo_click(todo)" v-for="todo in todos" :key="todo.id">
        <del v-if="todo.is_completed">{{todo.title}}</del>
        <strong v-else>{{todo.title}}</strong>
      </li>
    </ul>
  </div>
</template>

<script>
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
      }
    }
}
</script>

<style>

</style>