<template>
  <form @submit.prevent="onSubmit">
    <div class="d-flex">
      <div class="flex-grow-1 mr-2">
        <input
          class="form-control"
          type="text"
          v-model="todo"
          placeholder="Type New To-Do"
        />
      </div>
      <div>
        <button class="btn btn-primary" type="submit">Add</button>
      </div>
    </div>
    <div v-show="hasError" class="errorMessage">This Field Cannot Be Empty</div>
  </form>
</template>

<script setup>
import { ref, defineEmits } from "vue";

const hasError = ref(false);
const todo = ref("");

const onSubmit = () => {
  if (todo.value === "") {
    hasError.value = true;
  } else {
    defineEmits("add-todo", {
      id: Date.now(),
      subject: todo.value,
      completed: false,
    });
    todo.value = "";
    hasError.value = false;
  }
};
</script>

<style></style>
