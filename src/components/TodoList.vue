<template>
  <div class="">
    <input type="text" class="todo-input" placeholder="What needs me" v-model="newTodo" v-on:keyup.enter="addTodo"> 
    <div v-for="(todo,index) in todos" :key="todo.id" class="todo-item" >
      <div class="todo-item-left">
      <div v-if="!todo.editing" @dblclick="editTodo(todo)" class="todo-item-label">{{todo.title}}</div>
      <input v-else type="text" v-model="todo.title" class="todo-item-edit" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.escape="cancelEdit(todo)" v-focus>
      </div>
      <div class="remove-item" @click="removeTodo(index)">
        &times;
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'todo-list',
  data(){
    return {
      newTodo: '',
      idForTodo: 3,
      beforeEditCache: '',
      todos: [
        {
          'id': 1,
          'title': 'Show me Panties',
          'completed': false,
          'editing': false,

        },
        {
          'id': 2,
          'title': 'Take your clothes off',
          'completed': false,
          'editing': false,

        }
      ]
    }
  },
  directives: {
    focus: {
      inserted: function (el){
        el.focus()
      }
    }
  },
  methods: {
    addTodo() {
      if(this.newTodo.trim().length == 0){
        return 
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,

      })
      this.newTodo = ''
      this.idForTodo++
    },
    removetextTodo(index){
      this.todos.splice(index,1)
    },
    editTodo(todo){
      todo.editing = true
      this.beforeEditCache = todo.title
    },
    doneEdit(todo) {
      if(todo.title.trim() == ''){
        todo.title = this.beforeEditCache 
      }
      todo.editing = false
    },

    cancelEdit(todo) {
      todo.title = this.beforeEditCache
      todo.editing = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus{
    outline: 0;
  }
}

.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.remove-item {
  cursor: pointer;
  margin-left: 18px;
  &:hover {
    color: black;
  }
}
.todo-item-left{
  display: flex;
  align-items: center;
}
.todo-item-label{
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}
.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: 'Avenir', Helvetica,Arial, sans-serif;

  &:focus {
    outline: none;
  }
}
</style>
