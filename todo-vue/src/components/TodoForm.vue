<template>
  <form class="todo-form" @submit.prevent="todoCreate">
      <label for="title">todo:</label>
      <input v-model="title" type="text" id="title">
      <button type="submit">등록</button>
  </form>
</template>

<script>
export default {
    name:'TodoForm',
    data() {
      return {
        title : ''
      }
    },
    methods : {
      // form 이 제출되는 경우 해당 함수 실행
      todoCreate() {
        // 부모 컴포넌트로 이벤트 발생($emit)을 통해 이벤트 발생
        this.$emit('todoCreate-event',this.title)
        this.title = ''
      }
    }
}
</script>

<style>

</style>