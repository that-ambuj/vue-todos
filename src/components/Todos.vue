// script
<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import type { Ref } from "vue";

interface todo {
  readonly id: number;
  readonly text: string;
}

let id = 0;

const newTodo = ref("");
const todos: Ref<todo[]> = ref([]);

function addTodo(): void {
  if (newTodo.value !== "") {
    todos.value.push({ id: id++, text: newTodo.value });
    setTodos(todos.value);
    newTodo.value = "";
  }
}

function removeTodo(todo: todo): void {
  todos.value = todos.value.filter((t) => t !== todo);
  setTodos(todos.value);
}

function setTodos(todos: todo[]): void {
  window.localStorage.setItem("Todos", JSON.stringify(todos));
}

function getTodos(): todo[] {
  const todos: string = window.localStorage.getItem("Todos");
  return JSON.parse(todos);
}

onMounted(() => {
  todos.value = getTodos();
});

onUnmounted(() => {
  setTodos(todos.value);
  todos.value = [];
});
</script>

// template
<template>
  <div class="center">
    <h1>Todos</h1>
    <form @submit.prevent="addTodo">
      <input
        class="input"
        type="text"
        v-model="newTodo"
        placeholder="New Todo"
      />
    </form>

    <ul v-auto-animate="{ duration: 300 }" class="list">
      <li class="todo" v-for="todo in todos" key="todo.id">
        <span class="todo-text">{{ todo.text }}</span>
        <button class="delete" @click="removeTodo(todo)">Delete</button>
      </li>
    </ul>
  </div>
</template>

// styles
<style scoped>
.list {
  display: flex;
  flex-direction: column;
  place-items: left;
  margin-left: -30px;
}
.input {
  height: 30px;
  width: 250px;
  background-color: #1b1b1b;
  border-style: none;
  border-radius: 6px;
  padding: 2px 10px;
}

.input:focus {
  outline: 0px;
  background-color: #111;
}
.todo {
  font-size: 20px;
  margin: 8px 0px;
  display: flex;
  align-content: space-between;
  justify-content: space-between;
}

.todo-text {
}

.hidden {
  display: none;
}

.todo-text-done {
  margin-right: 30px;
  text-decoration: line-through;
}

.delete {
  font-family: "DM Sans";
  font-size: 16px;
  font-weight: 500;
  background-color: #ec3a2e;
  border-style: none;
  border-radius: 4px;
  width: 75px;
  color: black;
}

.delete:hover {
  background-color: #b92d24;
}
.delete:active {
  background-color: #85201a;
}
</style>
