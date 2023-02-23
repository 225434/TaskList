<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <h2>任务清单</h2>
        <!-- 
          <TheHeader :addTodo="addTodo"/> 
          使用绑定事件实现子组件向父组件传数据
        -->
        <TheHeader @addTodo="addTodo"/><hr>
        <!-- 使用事件总线，不需要在给TheList组件传值
             <TheList :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"/> 
        -->
        <TheList :todos="todos"/> 
        <TheFooter :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"/>
      </div>
    </div>
</div>

</template>

<script>
import TheHeader from './components/TheHeader.vue';
import TheList from './components/TheList.vue';
import TheFooter from './components/TheFooter.vue';

export default {
  name: 'App',
  data() {
    return {
      //由于todos是MyHeader组件和MyFooter组件都在使用，所以放在App中
      //初次使用没有缓存，取[]
      todos:JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  methods: {
    //添加一个todo
    addTodo(todoObj) {
      this.todos.unshift(todoObj)
    },
    //勾选或取消勾选
    checkTodo(id) {
      this.todos.forEach((todo)=>{
        if(todo.id === id) todo.completed = !todo.completed
      })
    },
    //修改一个todo
    updateTodo(id,target) {
      this.todos.forEach((todo)=>{
        if(todo.id === id) todo.target = target
      })
    },
    //删除一个事件
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id)
    },
    //全选或取消全选
    checkAllTodo(done) {
      this.todos.forEach((todo) => {
        todo.completed = done 
      })
    },
    //清除所有已完成的任务
    clearAllTodo() {
      this.todos = this.todos.filter(todo => !todo.completed)
    }
  },
  components: {
    TheHeader,TheList,TheFooter
  },
  //监视属性&&深度监视实现缓存效果
  watch: {
    todos: {
      deep:true,
      handler(value) {
        localStorage.setItem('todos',JSON.stringify(value))
      }
    }
  },
  //绑定事件总线
  mounted() {
    this.$bus.$on('checkTodo',this.checkTodo)
    this.$bus.$on('deleteTodo',this.deleteTodo)
    this.$bus.$on('updateTodo',this.updateTodo)
  },
  //解绑
  beforeDestroy() {
    this.$bus.$off('checkTodo')
    this.$bus.$off('deleteTodo')
    this.$bus.$off('updateTodo')
  }
}

</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-edit {
  color: #fff;
  background-color: seagreen;
  border: 1px solid green;
  margin-right: 5px;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn-edit:hover {
  color: #fff;
  background-color:darkgreen;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

</style>
