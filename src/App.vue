<template>
  <div id="appContainer">
    <h1>ToDo List</h1>
    <form @submit.prevent="addTodos()">
      <input v-model="newTodo" />
      <button class="waves-effect waves-light btn">Add ToDo</button>
    </form>
    <div id="todoList">
      <div
        :class="{ completed: todo.complete }"
        v-for="(todo, index) in todos"
        :key="index"
        @click="completedTodo(todo)"
      >
        <div class="row">
          <div class="col s12 m6">
            <div class="card teal teal lighten-1">
              <div class="card-content white-text">
                <span class="card-title">{{ todo.text }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <p v-if="todos.length === 0">Empty list, please enter some tasks</p>
    <button
      class="btn waves-effect waves-light red lighten-2"
      @click="removeTodos"
      v-if="todos.length !== 0"
    >
      Remove All Todos
    </button>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const newTodo = ref("");
    const initialLoadData = [
      {
        complete: false,
        text: "Create todo list functionality",
      },
    ];

    let storedTodos;
    localStorage.getItem("todos")
      ? (storedTodos = JSON.parse(localStorage.getItem("todos")))
      : (storedTodos = initialLoadData);
    const todos = ref(storedTodos);

    function addTodos() {
      if (newTodo.value !== "") {
        todos.value.push({
          complete: false,
          text: newTodo.value,
        });
        newTodo.value = "";
        updateStorage();
      }
    }

    function removeTodos() {
      todos.value.splice(0, todos.value.length);
      updateStorage();
    }

    function completedTodo(todo) {
      todo.complete = !todo.complete;
      updateStorage();
    }

    function updateStorage() {
      localStorage.setItem("todos", JSON.stringify(todos.value));
    }

    return {
      completedTodo,
      removeTodos,
      addTodos,
      todos,
      newTodo,
    };
  },
};
</script>

<style>

  #todoList {
    margin: 2%;
    padding: 0%;
  }
  .card-content:hover {
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 2);
  }
  #todoList div.completed {
    opacity: 0.5;
  }
  #appContainer {
    padding: 2%;
}

</style>
