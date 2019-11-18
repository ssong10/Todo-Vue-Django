<template>
  <div class="home">
    <TodoList />
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'
import TodoList from '@/components/TodoList.vue'

export default {
  name: 'home',
  components: {
    TodoList
  },
  data() {
    // 컴포넌트 에서는 반드시 data를 함수로 작성하고, object를 리턴한다
    return {
      todos : [],
    }
  },
  mounted() {
    //axios 요청
    axios.get('http://127.0.0.1:8000/api/v1/todos/')
    .then(response => {
      console.log(response) // 만약 console.log 에러가 나게 된다면, package.json-> "no-console-off"
    })
  }
}
</script>
