<template>
    <li>
        <label>
            <input type="checkbox" :checked="content.completed" @change="handleCheck(content.id)"/>
            <span v-show="!content.isEdit">{{ content.target }}</span>
            <input v-show="content.isEdit" type="text" :value="content.target" @blur="handleBlur(content,$event)" ref="autoFocus">
        </label>
        <button class="btn btn-danger"  @click="handleDelete(content.id)">删除</button>
        <button v-show="!content.isEdit" class="btn btn-edit" @click="handleEdit(content)">编辑</button>

    </li>
</template>

<script>
    export default {
        name:'TheItem',
        //使用事件总线直接传值，不再需要checkTodo和deleteTodo
        //props:['content','checkTodo','deleteTodo'],
        props:['content'],
        methods:{
          handleCheck(id) {
            //通知App组件将对应的todo对象的done值取反
            //this.checkTodo(id)
            this.$bus.$emit('checkTodo',id)
          },
          //实现删除效果
          handleDelete(id) {
            if(confirm('确认删除？')) 
              //this.deleteTodo(id)
              this.$bus.$emit('deleteTodo',id)
          },
          //实现编辑
          handleEdit(todo) {
            //非null判断
            if(Object.prototype.hasOwnProperty.call(todo, 'isEdit')) {
              todo.isEdit = true
            } else {
              this.$set(todo,'isEdit',true)
            }
            //输入框自动获取焦点(在DOM更新之后)
            this.$nextTick(function(){
              this.$refs.autoFocus.focus()
            })
          },
          //失去焦点时完成编辑，回调（真正完成修改逻辑）
          handleBlur(todo,e) {
            todo.isEdit = false
            this.$bus.$emit('updateTodo',todo.id,e.target.value)
          }
        }
    }
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li label input {
    padding: 0.5em;
    border-radius: 5px;
    border: 1px solid lightgray; 
  }

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color:#ddd;
}

li:hover button {
  display: block;
}
</style>