<template>
  <div class="container">
    <h2>To-Do List</h2>
    <TodoSimpleForm @add-todo="addTodo" />

    <div v-if="!todos.length" class="mt-2">추가된 Todo가 없습니다</div>
    <div
      class="card mt-2"
      v-for="(todoList, index) in todos"
      :key="todoList.id"
    >
      <div class="card-body p-2 d-flex align-items-center">
        <div class="form-check flex-grow-1">
          <input
            type="checkbox"
            class="form-check-input"
            v-model="todoList.completed"
          />
          <label
            class="form-check-label"
            :class="{ completedTodo: todoList.completed }"
            >{{ todoList.subject }}</label
          >
        </div>
        <div>
          <button class="btn btn-danger btn-sm" @click="deleteTodo(index)">
            Delete
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from "vue";
import TodoSimpleForm from "./components/TodoSimpleForm.vue";

const todos = reactive([]);

const addTodo = (todo) => {
  console.log(todo);
};

const deleteTodo = (index) => {
  todos.splice(index, 1);
};
</script>

<style>
.errorMessage {
  color: red;
}

.completedTodo {
  text-decoration: line-through;
  color: gray;
}
</style>
