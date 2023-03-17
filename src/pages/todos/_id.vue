<template>
  <h1>To-Do Page</h1>
  <div v-if="loading">loading....</div>
  <form v-else @submit.prevent="updateTodo">
    <div class="row">
      <div class="col-6">
        <div class="form-group">
          <label>Subject</label>
          <input
            type="text"
            class="form-control mt-2 mb-2"
            v-model="todo.subject"
          />
        </div>
      </div>
      <div class="col-6">
        <div class="form-group">
          <label>Status</label>
          <div>
            <button
              class="mt-2 btn"
              type="button"
              :class="todo.completed ? 'btn-success' : 'btn-danger'"
              @click.stop="toggleTodoStatus"
            >
              {{ todo.completed ? "Completed" : "Incomplete" }}
            </button>
          </div>
        </div>
      </div>
    </div>
    <button type="submit" class="btn btn-primary" :disabled="todoUpdated">
      Save
    </button>
    <button
      type="button"
      class="btn btn-outline-dark ms-2"
      @click="moveTodoListPage"
    >
      Cancel
    </button>
  </form>
  <Toast v-show="showToast" :toast-message="toastMessage" />
</template>

<script setup>
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
import { ref, computed } from "vue";
import _ from "lodash";
import Toast from "../../components/Toast.vue";

const route = useRoute();
const router = useRouter();
const todo = ref(null);
const originalTodo = ref(null);
const loading = ref(true);
const showToast = ref(false);
const toastMessage = ref("");
const todoId = route.params.id;
const todoUpdated = computed(() => {
  return _.isEqual(todo.value, originalTodo.value);
});

const getTodo = async () => {
  const res = await axios.get(`http://localhost:3000/todos/${todoId}`);
  todo.value = { ...res.data };
  originalTodo.value = { ...res.data };
  loading.value = false;
};

getTodo();

const triggerToast = (message) => {
  showToast.value = true;
  toastMessage.value = message;
  setTimeout(() => {
    showToast.value = false;
  }, 1500);
};

const updateTodo = async () => {
  await axios.patch(`http://localhost:3000/todos/${todoId}`, {
    subject: todo.value.subject,
  });
  getTodo();
  triggerToast("Successfully Saved");
};

const toggleTodoStatus = async () => {
  await axios.patch(`http://localhost:3000/todos/${todoId}`, {
    completed: !todo.value.completed,
  });
  getTodo();
};

const moveTodoListPage = () => {
  router.push({ name: "Todos" });
};
</script>

<style></style>
