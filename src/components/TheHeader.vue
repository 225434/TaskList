<template>
    <div class="todo-header">
        <input type="text" placeholder="请输入您的任务名称，按回车键确认" v-model="target" @keyup.enter="add"/>
    </div>
</template>

<script>

import {nanoid} from 'nanoid'

  export default {
      name:'TheHeader',
      data() {
        return {
          target:''
        }
      },
      methods: {
        add() {
          //校验数据
          if(!this.target.trim()) return alert('输入不能为空！')
          //将数据包装成对象
          const todoObj = {id:nanoid(),target:this.target,completed:false}
          // //添加对象到App的todo数组
          // this.addTodo(todoObj)
          //使用绑定事件
          this.$emit('addTodo',todoObj)
          //清空输入
          this.target = ''
        }
      }
  }
</script>

<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}
</style>