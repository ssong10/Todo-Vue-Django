<template>
  <div class="home">
    <TodoForm @todoCreate-event="todoCreate" />
    <TodoList :todos="todos" />
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
import TodoList from '@/components/TodoList.vue'
import TodoForm from '@/components/TodoForm.vue'

export default {
  name: 'home',
  components: {
    TodoList,
    TodoForm
  },
  data() {
    // 컴포넌트 에서는 반드시 data를 함수로 작성하고, object를 리턴한다
    return {
      todos : [],
    }
  },
  methods : {
    todoCreate(title) {
      console.log('==부모컴포넌트==')
      console.log(title)
      // const data = {
      //   title:title,
      //   is_completed : false,
      //   user : 1
      // }

      // request.POST 인 경우는 반드시 formData!
      const formData = new FormData()
      formData.append('title',title)
      formData.append('user',1)
      axios.post('http://127.0.0.1:8000/api/v1/todos/',formData)
      .then(response => {
        this.todos.push(response.data)
      })
      .catch(error =>{
        console.log(error)
      })
    },
    getTodos() {
      //axios 요청
      axios.get('http://127.0.0.1:8000/api/v1/todos/')
      .then(response => {
        this.todos = response.data
        console.log(response) // 만약 console.log 에러가 나게 된다면, package.json-> "no-console-off"
      })
      .catch(error =>{
      console.log(error)
      })
    }
  },
  mounted() {
    this.getTodos()
  }
}
</script>
