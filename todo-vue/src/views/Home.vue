<template>
  <div class="home">
    <TodoForm @todoCreate-event="todoCreate" />
    <!-- <TodoList :todos="all_todos" /> -->
  <hr>
    <TodoList :todos="todos" />
  </div>
</template>

<script>
// @ is an alias to /src
import router from '../router'
import axios from 'axios'
import {mapGetters} from 'vuex' // mapGetters 하나만 가져온다
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
      // all_todos : [],
      todos : [],
    }
  },
  methods : {
    todoCreate(title) {
      // this.$session.start()
      // const token = this.$session.get('jwt')
      // const options = {
      //   headers : {
      //     Authorization : `JWT ${token}`
      //   }
      // }

      // const data = {
      //   title:title,
      //   is_completed : false,
      //   user : jwtDecode(token).user_id
      // }

      // request.POST 인 경우는 반드시 formData!
      const formData = new FormData()
      formData.append('title',title)
      formData.append('user', this.user)
      axios.post('http://127.0.0.1:8000/api/v1/todos/',formData,this.options)
      .then(response => {
        this.todos.push(response.data)
      })
      .catch(error =>{
        console.log(error)
      })
    },
    getTodos() {
      // 전체 Todo 받기 
      //axios 요청
      // axios.get('http://127.0.0.1:8000/api/v1/todos/',options)
      // .then(response => {
      //   this.all_todos = response.data
      //   console.log(response) // 만약 console.log 에러가 나게 된다면, package.json-> "no-console-off"
      // })
      // .catch(error =>{
      //   console.log(error)
      // })
      // 한 아이디
      axios.get(`http://127.0.0.1:8000/api/v1/users/${this.user}/`,this.options)
      .then(response => {
        this.todos = response.data.todo_set
        console.log(response)
      })
      .catch(error =>{
        console.log(error)
      })    
    },
    isLogin() {
      this.$session.start()
      // jwt 가 없다면 => token이 없다면, 비로그인이면 로그인페이지로 이동
      if (!this.$session.has('jwt')) {
        router.push('/login')
      } else {
        this.$store.dispatch('login',this.$session.get('jwt'))
      }

    }
  },
  mounted() {
    this.isLogin() // 로그인이 되어있으면
    this.getTodos() // 가져온다
  },
  computed : {
    // spread 문법 '...'
    ...mapGetters([
      'options',
      'user'
    ])
    // options() {
    //   return this.$store.getters.options
    // },
    // user() {
    //   return this.$store.getters.user
    // }
  }
}
</script>
