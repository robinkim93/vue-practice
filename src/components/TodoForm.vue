<template>
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
      <div v-if="edting" class="col-6">
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
      <div class="col-12">
        <div class="form-group">
          <label>Content</label>
          <textarea
            v-model="todo.content"
            class="form-control mt-2 mb-2"
            cols="30"
            rows="10"
          ></textarea>
        </div>
      </div>
    </div>
    <button type="submit" class="btn btn-primary" :disabled="todoUpdated">
      {{ edting ? "Save" : "Create" }}
    </button>
    <button
      type="button"
      class="btn btn-outline-dark ms-2"
      @click="moveTodoListPage"
    >
      Cancel
    </button>
  </form>
  <Toast
    v-show="showToast"
    :toast-message="toastMessage"
    :toastAlertType="toastAlertType"
  />
</template>

<script setup>
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
import { ref, computed, defineProps } from "vue";
import _ from "lodash";
import Toast from "@/components/Toast.vue";
import { useToast } from "@/composables/useToast";

const route = useRoute();
const router = useRouter();
const todo = ref({
  subject: "",
  content: "",
});
const originalTodo = ref(null);
const loading = ref(false);
const todoId = route.params.id;
const todoUpdated = computed(() => {
  return _.isEqual(todo.value, originalTodo.value);
});
const { showToast, toastAlertType, toastMessage, triggerToast } = useToast();
const props = defineProps({ edting: { type: Boolean, default: false } });

const getTodo = async () => {
  try {
    loading.value = true;

    const res = await axios.get(`http://localhost:3000/todos/${todoId}`);

    todo.value = { ...res.data };
    originalTodo.value = { ...res.data };

    loading.value = false;
  } catch (error) {
    console.log(error);
    triggerToast("Something went wrong", "danger");
  }
};

if (props.edting) {
  getTodo();
}

const updateTodo = async () => {
  try {
    // if(props.edting) {
    //   await axios.patch(`http://localhost:3000/todos/${todoId}`, {
    //   subject: todo.value.subject,
    // });
    // } else {
    //   await axios.post('http://localhost:3000/todos', {
    //     subject:
    //   })
    // }
    await axios.patch(`http://localhost:3000/todos/${todoId}`, {
      subject: todo.value.subject,
    });
    getTodo();
    triggerToast("Successfully Saved");
  } catch (error) {
    console.log(error);
    triggerToast("Something went wrong", "danger");
  }
};

const toggleTodoStatus = async () => {
  try {
    await axios.patch(`http://localhost:3000/todos/${todoId}`, {
      completed: !todo.value.completed,
    });
    getTodo();
    triggerToast("Successfully Saved");
  } catch (error) {
    console.log(error);
    triggerToast("Something went wrong", "danger");
  }
};

const moveTodoListPage = () => {
  router.push({ name: "Todos" });
};
</script>

<style></style>
