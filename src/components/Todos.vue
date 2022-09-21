// script
<script setup lang="ts">
import { ref } from "vue";
import type { Ref } from "vue";

interface todo {
  readonly id: number;
  readonly text: string;
}

let id = 0;

const newTodo = ref("");
const todos: Ref<todo[]> = ref([
  { id: id++, text: "Learn Go" },
  { id: id++, text: "Learn Vue" },
  { id: id++, text: "Complete CS50 Course" },
  { id: id++, text: "Learn React" },
]);

function addTodo(): void {
  if (newTodo.value !== "") {
    todos.value.push({ id: id++, text: newTodo.value });
    newTodo.value = "";
  }
}

function removeTodo(todo: todo): void {
  todos.value = todos.value.filter((t) => t !== todo);
}
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

    <ul v-auto-animate="{ duration: 400 }" class="list">
      <li class="todo" v-for="todo in todos" key="todo.id">
        <span class="todo-text">{{ todo.text }}</span>
        <button class="delete" @click="removeTodo(todo)">Delete</button>
      </li>
    </ul>
  </div>
</template>

// styles
<style scoped>
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
  margin-right: 30px;
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
