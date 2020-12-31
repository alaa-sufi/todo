<template>
  <div id="app">
    <div class="container">
      <h1>My Todos</h1>
      <form @submit.prevent>
        <input type="text" placeholder="Create a new to-do..." autofocus v-model.trim="newtodo"/>
        <button @click="addtodo()" class="add">Add</button>
      </form>
      <ul class="todo">
        <li v-for="(todo,i) in todos" :key="todo-name">
          <span v-if="!todo.edit" class="todo-name" @click="todo.finish = 1 - todo.finish":class="{'finish':todo.finish}"> {{todo.name}}</span>
          <input v-else  autofocus class="edit-input" type="text" v-model="todos[i].name"/>
          <div class="icons">
            <button class="edit" @click="todo.edit=!todo.edit"><i class="fa fa-edit"></i></button>
            <button class="delete" @click="deletetoto(todo)"><i class="fas fa-trash"></i></button>
          </div>
        </li>
      </ul>
      <div class="footer">
        <span>All Todos : {{ todos.length == 0 ? "No Todos" : todos.length }}</span>
        <button v-show="todos.length > 1" class="delete-all" @click="deleteAll">Delete All</button>
      </div>
      <h4 class='made'>Made With <i class="fa fa-heart"></i> By Alaa Sufi © 2020</i></h4>
    </div>
    
  </div>
</template>
<script>
export default {
  data() {
    return {
      todos: [],
      newtodo: ""
    }
  },
  methods: {
    addtodo() {
      var newtodosave = this.newtodo
      this.todos.length < 1
        ? this.todos.push({ name: this.newtodo, edit: 0, finish: 0 })
        : this.todos.some(function (x) {
            return x.name == newtodosave
          }) || this.newtodo.length == 0
        ? this.todos
        : this.todos.push({ name: this.newtodo, edit: 0, finish: 0 })
      this.newtodo = ""
    },
    deletetoto(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1)
    },
    deleteAll() {
      this.todos.splice(0)
    }
  },
  mounted() {
    if (localStorage.todosSave) {
      this.todos = JSON.parse(localStorage.todosSave)
    }
  },
  watch: {
    todos(newName) {
      localStorage.todosSave = JSON.stringify(newName)
    }
  }
}
</script>
<style lang="scss">
* {
  box-sizing: border-box;
}
#app {
  font-family: Lato, sans-serif;
  color: #2c3e50;
}
.container {
  width: 800px;
  margin: auto;
  margin-top: 1rem;
  @media (max-width: 800px) {
    & {
      width: 100%;
    }
  }
}
h1 {
  margin-top: 2rem;
  margin-bottom: 2rem;
}
form {
  display: flex;

  input {
    padding: 10px;
    border: 1px solid #bbb;
    border-radius: 5px 0px 0px 5px;
    font-size: 20px;
    width: 90%;
  }
  .add {
    background-color: #0cca8c;
    padding: 5px 10px;
    color: white;
    outline: none;
    border: none;
    width: 10%;
    border-radius: 0px 5px 5px 0px;
    cursor: pointer;
  }
}

ul.todo {
  border: 1px solid #bbb;
  border-radius: 5px;
  padding: 0;
  list-style: none;
  margin-bottom: 0;
  li {
    display: flex;
    justify-content: space-between;
    padding: 1rem 2rem;
    user-select: none;
    cursor: pointer;
    .todo-name {
      &.finish {
        text-decoration: line-through;
      }
    }
    &:not(:last-child) {
      border-bottom: 1px solid #bbb;
    }
    & > span {
      font-size: 20px;
      padding: 0;
      line-height: 1.5;
    }
  }
}
.edit-input {
  font-size: 20px;
  padding: 0;
  color: #2c3e50;
}
.icons {
  .edit,
  .delete {
    all: unset;
    cursor: pointer;
    width: 35px;
    text-align: center;
    height: 35px;
    border-radius: 5px;
  }
  .edit {
    color: #007bff;
    background-color: #fff;
    &:hover {
      background-color: #007bff;
      color: #fff;
    }
  }
  .delete {
    color: #dc3545;
    background-color: #fff;
    &:hover {
      background-color: #dc3545;
      color: #fff;
    }
  }
}
.footer {
  display: flex;
  justify-content: space-between;
  span {
    padding: 15px;
    border: 1px solid #bbb;
    border-radius: 5px;
  }
  .delete-all {
    background-color: #dc3545;
    padding: 15px;
    color: white;
    outline: none;
    border: none;
    border-radius: 5px;
    display: block;
    cursor: pointer;
  }
}
.made {
  text-align: center;
  i {
    color: #dc3545;
  }
}
</style>
