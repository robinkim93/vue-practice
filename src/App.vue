<template>
  <div class="container">
    <h2>To-Do List</h2>
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
      <div v-show="hasError" class="errorMessage">
        This Field Cannot Be Empty
      </div>
    </form>
    <div class="card mt-2" v-for="todo in todos" :key="todo.id">
      <div class="card-body p-2">
        <div class="form-check">
          <input
            type="checkbox"
            class="form-check-input"
            v-model="todo.completed"
          />
          <label class="form-check-label">{{ todo.subject }}</label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from "vue";
export default {
  setup() {
    const hasError = ref(false);
    const todo = ref("");
    const todos = reactive([]);

    const onSubmit = () => {
      if (todo.value === "") {
        hasError.value = true;
      } else {
        todos.push({
          id: Date.now(),
          subject: todo.value,
          completed: false,
        });
        todo.value = "";
        hasError.value = false;
      }
    };

    return { todo, todos, hasError, onSubmit };
  },
};
</script>

<style>
.errorMessage {
  color: red;
}
</style>
