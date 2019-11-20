<template>
  <form @submit.prevent="login" >
    <label for="id">사용자 이름 : </label>
    <input v-model='credentials.username' id="id" type="text"><br>
    <label for="password">비밀번호 : </label>
    <input v-model='credentials.password' id="password" type="password"><br>
    <input type="submit" value="로그인">
  </form>
</template>

<script>
import axios from 'axios'
// 특정 폴더명으로 경로가 끝나게 되면, 폴더 내부의 index.js 를 뜻함.
import router from '../router'
export default {
    name : "LoginForm",
    data() {
        return {
            credentials : {}
        }
    },
    methods : {
        login(){
            axios.post('http://127.0.0.1:8000/api-token-auth/',this.credentials)
            .then(response => {
                const token = response.data.token
                this.$session.start()
                this.$session.set('jwt',token)
                // vuex actions 호출 -> dispatch
                this.$store.dispatch('login',token)
                router.push('/')
            })
            .catch(error =>{
                console.log(error)
            })
        }
    }
}
</script>

<style>

</style>