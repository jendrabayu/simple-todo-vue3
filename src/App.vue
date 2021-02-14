
<template>
  <div class="container">
    <img class="logo" src="./assets/logo.png" alt="Vue Logo" />

    <input
      class="todo-input"
      type="text"
      placeholder="Whats needs to be done..."
      v-model="newTodo"
      @keyup.enter="addTodo"
    />

    <div>
      <transition-group
        name="fade"
        leave-active-class="animate__animated  animate__fadeOutDown"
        enter-active-class="animate__animated  animate__fadeInUp"
      >
        <div v-for="(todo, index) in todosFiltered" :key="todo.id">
          <todo-item
            :todo="todo"
            :checkAll="!anyRemaining"
            :index="index"
            @removeTodo="removeTodo"
            @doneEdit="doneEdit"
          />
        </div>
      </transition-group>

      <div class="extra-container">
        <todo-check-all
          :anyRemaining="anyRemaining"
          :anyTodos="anyTodos"
          @checkAllTodo="checkAllTodo"
        />
        <todo-item-remaining :remaining="remaining" />
      </div>

      <div class="extra-container">
        <todo-filtered @filterChanged="changeFilter" />
        <todo-clear-completed
          :anyCompleted="anyCompleted"
          @clearCompleted="clearCompleted"
        />
      </div>
    </div>
  </div>
</template>

<script>
import TodoCheckAll from "./components/TodoCheckAll.vue";
import TodoClearCompleted from "./components/TodoClearCompleted.vue";
import TodoFiltered from "./components/TodoFiltered.vue";
import TodoItem from "./components/TodoItem.vue";
import TodoItemRemaining from "./components/TodoItemRemaining.vue";
export default {
  components: {
    TodoItem,
    TodoItemRemaining,
    TodoCheckAll,
    TodoClearCompleted,
    TodoFiltered,
  },
  data() {
    return {
      newTodo: "",
      todos: [],
      idForTodo: 1,
      beforeEditCache: "",
      filter: "all",
    };
  },
  created() {
    document.title = "Simple Todo";
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim("").length === 0) {
        return;
      }

      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
        editing: false,
      });

      this.idForTodo++;
      this.newTodo = "";
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    doneEdit({ index, todo }) {
      this.todos.splice(index, 1, todo);
    },
    checkAllTodo(event) {
      this.todos.forEach((todo) => (todo.completed = event.target.checked));
    },
    clearCompleted() {
      this.todos = this.todos.filter((todo) => !todo.completed);
    },
    changeFilter(filter) {
      this.filter = filter;
    },
  },
  computed: {
    remaining() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining !== 0;
    },
    anyTodos() {
      return this.todos.length !== 0;
    },
    todosFiltered() {
      switch (this.filter) {
        case "all":
          return this.todos;
        case "active":
          return this.todos.filter((todo) => !todo.completed);
        case "completed":
          return this.todos.filter((todo) => todo.completed);
        default:
          return this.todos;
      }
    },
    anyCompleted() {
      return this.todos.filter((todo) => todo.completed).length > 0;
    },
  },
};
</script>


<style>
@import url(https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css);
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

:root {
  --animate-duration: 300ms;
  --animate-delay: 0.9s;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  margin-bottom: 100px;
}

.logo {
  height: 100px;
  display: block;
  margin: 20px auto;
}

.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
  box-sizing: border-box;
}

.todo-input:focus {
  outline: none;
}

.todo-item {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 14px;
}

.todo-item-left {
  display: flex;
  align-items: center;
}

.todo-item-label {
  padding: 10px;
  font-size: 16px;
  margin-left: 10px;
}

button {
  border: none;
  margin-right: 5px;
  cursor: pointer;
  padding: 3px 5px;
}

.remove-item {
  cursor: pointer;
  margin-left: 7px;
}

.remove-item:hover {
  color: red;
}

.todo-item-edit {
  font-size: 18px;
  width: 100%;
  color: #2c3e50;
  padding: 10px;
  margin-left: 12px;
  border: 1px solid #ccc;
}

.extra-container {
  display: flex;
  font-size: 13px;
  justify-content: space-between;
  padding-top: 14px;
  margin-bottom: 14px;
  align-items: center;
  border-top: 1px solid lightgray;
}
.active {
  background-color: lightgreen;
}
</style>
