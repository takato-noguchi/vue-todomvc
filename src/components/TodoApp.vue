<template>
<div class="container">
  <div class="add-container">
    <h2>Todo App</h2>
    <p>
      <input type="text" v-model="todoText" placeholder="Todoを入力" class="input-form">
      <button @click="addTodo" class="btn btn-primary btn-sm">追加</button>
    </p>
    <p>
      Todo{{todos.length}}件中残り{{remaining}}件
      <button  @click="archive" class="btn btn-danger btn-sm archive-btn">選択したものを削除する</button>
      <button @click="removeAllTodo" class="btn btn-dark btn-sm">すべて削除する</button>
    </p>
  </div>

  <div class="list-container">
    <h3>Todo List</h3>
    <ul class="list-unstyled">
      <li 
        v-for="todo in todos"
        :key="todo.id"
      >
      <label>
        <input type="checkbox" v-model="todo.done" class="checkbox">
        <span :class="{'done': todo.done}">{{todo.text}}</span>
      </label>
        <button @click="removeTodo(todo)" class="btn btn-danger btn-sm remove-btn">削除する</button>
      </li>
    </ul>
  </div>
</div>
</template>

<script>
var STORAGE_KEY = 'todos-vuejs-2.0',
  todoStorage = {
  fetch: function () {
    var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]')
    todos.forEach(function (todo, index) {
      todo.id = index
    })
    todoStorage.uid = todos.length
    return todos
  },
  save: function (todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
  }
}
export default {
  name: "TodoApp",
  data() {
    return {
      todoText: '',
      todos: todoStorage.fetch(),
      uid: 0
    }
  },
  watch: {
    todos: {
      handler(todos) {
        todoStorage.save(todos);
      },
      deep: true
    }
  },
  computed: {
    remaining() {
      const count =
        this.todos.reduce((count, todo) =>
          count = (todo.done) ? count : ++count
        , 0);
      return count;
    }
  },
  methods: {
    addTodo() {
      const newTodo = this.todoText.trim();
      this.uid += 1
      if (!newTodo) {
        return;
      }
      this.todos.push({
        id: this.uid,
        text: newTodo, 
        done: false
      })
      localStorage.setItem('uid',this.uid)
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((_todo) => _todo !== todo);
    },
    archive() {
      this.todos = this.todos.filter((todo) => !todo.done);
    },
    removeAllTodo() {
      this.todos = []
    }
  }
}
</script>

<style scoped>
.input-form {
  width: 300px;
}
.checkbox {
  margin: 8px 8px 8px 8px;
}
.remove-btn {
  margin: 4px 8px 4px 8px;
}
.done {
  text-decoration: line-through;
  color: grey;
}
.archive-btn {
  margin: 0px 4px 0px 4px;
}
</style>