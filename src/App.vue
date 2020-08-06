<template>
  <div id="app">
    <h2>Vue Todo</h2>
    <form @submit.prevent="addTodo" class="mb-1">
      <input
        type="text"
        name="newTodo"
        v-model="newTodo"
        placeholder="Add new todo"
        class="newTodoInput"
      />
      <button class="newTodoButton">Add</button>
    </form>
    <div v-for="todo in todoList" :key="todo.id" class="listItem">
      <p
        v-if="!todo.editing"
        v-bind:class="{ done: todo.done }"
        @click="toggleDone(todo)"
      >
        {{ todo.task }}
      </p>
      <input
        v-else
        v-model="todo.task"
        @keydown.enter="toggleEdit(todo)"
        class="editInput"
        :ref="todo.id"
        v-focus
      />
      <div class="editDeleteContainer">
        <button class="actionButton" @click="delTodo(todo)">🗑️</button>
        <button class="actionButton" @click="toggleEdit(todo)">✏️</button>
      </div>
    </div>
    <div v-if="todoList.length" class="listButtonContainer">
      <button class="listButton" @click="allComplete">
        Mark all as completed
      </button>
      <button class="listButton" @click="deleteAllComplete">
        Delete all completed
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      newTodo: "",
      todoList: []
    };
  },
  methods: {
    addTodo: function() {
      if (this.newTodo.length) {
        this.todoList.push({
          task: this.newTodo,
          done: false,
          editing: false,
          id: Math.random() * 100000000000000000
        });
        this.newTodo = "";
      }
    },
    delTodo: function(deletedTodo) {
      this.todoList = this.todoList.filter(todo => todo !== deletedTodo);
    },
    toggleDone: function(todo) {
      todo.done = !todo.done;
    },
    toggleEdit: function(todo) {
      todo.editing = !todo.editing;
    },
    allComplete: function() {
      this.todoList.forEach(todo => (todo.done = true));
    },
    deleteAllComplete: function() {
      this.todoList = this.todoList.filter(todo => todo.done === false);
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.done {
  text-decoration: line-through;
}

.listItem {
  max-width: 300px;
  width: 100%;
  display: flex;
  justify-content: space-between;
  cursor: pointer;
  align-items: center;
  padding: 0.25rem 1rem;
  background-color: #ebebeb;
  border: 1px solid #c5c5c5;
  border-radius: 0.25rem;
  margin: 0.25rem 0;
  min-height: 62px;
  text-align: left;
}

.mb-1 {
  margin-bottom: 1rem;
}

.editInput {
  margin: 0.5rem 0;
  background-color: #ebebeb;
  border-radius: 0.25rem;
  padding: 0.5rem 0;
  border: none;
  font-size: 16px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.actionButton {
  width: 30px;
  height: 30px;
  border: none;
  margin: 0 0.25rem;
  border-radius: 0.25rem;
  background-color: #f7f7f7;
  cursor: pointer;
}

.actionButton:hover {
  background-color: #fff;
  border: 1px solid #c5c5c5;
}

form {
  width: 300px;
  display: flex;
  height: 62px;
}

.newTodoInput {
  display: flex;
  flex-grow: 1;
  padding: 1rem;
  background-color: #ebebeb;
  border: none;
  border-bottom-left-radius: 0.5rem;
  border-top-left-radius: 0.5rem;
  font-size: 16px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.newTodoButton {
  width: 62px;
  border: none;
  background-color: #3ca967;
  color: white;
  font-size: 16px;
  border-top-right-radius: 0.5rem;
  border-bottom-right-radius: 0.5rem;
  cursor: pointer;
}

.newTodoButton:hover {
  background-color: #4bc57c;
}

.editDeleteContainer {
  min-width: 76px;
}

.listButtonContainer {
  display: flex;
  width: 300px;
  justify-content: space-between;
  margin-top: 1rem;
}

.listButton {
  display: flex;
  padding: 0.5rem;
  background-color: #ebebeb;
  border: none;
  border-radius: 0.25rem;
  font-size: 12px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  cursor: pointer;
}

.listButton:hover {
  background-color: #3ca967;
  color: white;
}

.listButton:focus {
  background-color: #4bc57c;
}
</style>
