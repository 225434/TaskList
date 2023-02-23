<template>
    <ul class="todo-main">
      <!-- 
        使用事件总线，不再经过TheList组件
        <TheItem v-for="todoObj in todos" :key="todoObj.id" :content="todoObj" :checkTodo="checkTodo" :deleteTodo="deleteTodo"/>
      -->
          <TheItem v-for="todoObj in todos" :key="todoObj.id" :content="todoObj"/>
    </ul>
</template>

<script>

import TheItem from './TheItem.vue';

export default {
    name:'TheList',
    components: {TheItem},
    // 使用事件总线，TheList组件不再需要接收checkTodo和deleteTodo
    // props: ['todos','checkTodo','deleteTodo']
    props: ['todos']
}
</script>

<style scoped>
/*main*/
.todo-main {
  margin-left: 0;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0;
}

</style>