<template>
    <div class="todo-footer" v-show="total">
        <label>
            <!-- <input type="checkbox" :checked="allDone" @change="checkAll"/> -->
          <input type="checkbox" v-model="allDone"/>
        </label>
        <span>
            <span>已完成{{ totalDone }}</span>/全部{{ total }}
        </span>
        <button class="btn btn-danger" @click="clearAll">清除已完成的任务</button>
    </div>
</template>

<script>
export default {
    name:'TheFooter',
    props:['todos'],
    computed:{
      total() {
        return this.todos.length
      },
      totalDone() {
        return this.todos.reduce((pre,todo) => pre + (todo.completed ? 1 : 0) ,0)
      },
      // allDone() {
      //   return this.total >   0 && this.total === this.totalDone
      // }
      
      //使用计算属性实现
      allDone: {
        get() {
          return this.total > 0 && this.total === this.totalDone
        },
        set(value) {
          // this.checkAllTodo(value)
          //使用绑定事件
          this.$emit('checkAllTodo',value)
        }
      }
    },
    methods: {
      // //全选或全不选
      // checkAll(e) {
      //   this.checkAllTodo(e.target.checked)
      // }

      //清除已完成的任务
      clearAll() {
        //this.clearAllTodo()
        //使用绑定事件
        if(this.totalDone !== 0)
          this.$emit('clearAllTodo')
        else
          alert('目前没有完成的任务！')
      }
    }
}
</script>

<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>