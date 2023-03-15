<template>
  <div
    class="card mt-2"
    v-for="(todoList, index) in props.todos"
    :key="todoList.id"
  >
    <div class="card-body p-2 d-flex align-items-center">
      <div class="form-check flex-grow-1">
        <input
          type="checkbox"
          class="form-check-input"
          :checked="todoList.completed"
          @change="toggleTodo(index)"
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
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({ todos: { type: Array, required: true } });
const emits = defineEmits(["toggle-todo", "delete-todo"]);

const toggleTodo = (index) => {
  emits("toggle-todo", index);
};

const deleteTodo = (index) => {
  emits("delete-todo", index);
};
</script>

<style>
.completedTodo {
  text-decoration: line-through;
  color: gray;
}
</style>
