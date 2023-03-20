<template>
  <div class="d-flex justify-content-between mb-2 mt-2">
    <h2>To-Do List</h2>
    <button class="btn btn-primary" @click="createTodo">Create Todo</button>
  </div>
  <input
    class="form-control"
    type="text"
    v-model="searchText"
    placeholder="Search your Todo"
    @keyup.enter="searchTodo"
  />
  <hr />
  <!-- 명시한 이벤트가 발생했을 때 실행 될 로직을 추가해준다. -->
  <div v-if="!todos.length" class="mt-2">표시할 Todo가 없습니다</div>
  <TodoList
    :todos="todos"
    @toggle-todo="toggleTodo"
    @delete-todo="deleteTodo"
  />
  <div>
    <hr />
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item" v-if="currentPage !== 1">
          <a
            style="cursor: pointer"
            class="page-link"
            @click="getTodos(currentPage - 1)"
            >Previous</a
          >
        </li>
        <li
          class="page-item"
          :class="currentPage === page ? 'active' : ''"
          v-for="page in totalPages"
          :key="page"
        >
          <a
            style="cursor: pointer"
            class="page-link"
            @click="getTodos(page)"
            >{{ page }}</a
          >
        </li>
        <li class="page-item" v-if="currentPage !== totalPages">
          <a
            style="cursor: pointer"
            class="page-link"
            @click="getTodos(currentPage + 1)"
            >Next</a
          >
        </li>
      </ul>
    </nav>
  </div>
  <Toast
    v-show="showToast"
    :toast-message="toastMessage"
    :toastAlertType="toastAlertType"
  />
</template>

<script setup>
import { computed, ref, watch } from "vue";
import { useRouter } from "vue-router";
import axios from "axios";
import TodoList from "../../components/TodoList.vue";
import Toast from "../../components/Toast.vue";
import { useToast } from "@/composables/useToast";

const todos = ref([]);
const searchText = ref("");
const error = ref("");
const totalTodos = ref(0);
const currentPage = ref(1);
const limit = 5;
const { showToast, toastAlertType, toastMessage, triggerToast } = useToast();
const router = useRouter();

// 내부에 있는 Reactive 값이 변할 때마다 새로 업데이트 되도록 computed를 사용.
const totalPages = computed(() => {
  return Math.ceil(totalTodos.value / limit);
});

let timeout = null;

const searchTodo = () => {
  clearTimeout(timeout);
  getTodos();
};

watch(searchText, () => {
  clearTimeout(timeout);
  timeout = setTimeout(() => {
    getTodos();
  }, 1000);
});

// emit으로 올라온 이벤트가 발생했을 때 실행 될 함수를 정의해줬다
// 이 때, 인자를 하나 받아오게 되면 자식 컴포넌트에서 올려준 데이터를 받을 수 있다.

const getTodos = async (page = currentPage.value) => {
  currentPage.value = page;
  try {
    const res = await axios.get(
      `http://localhost:3000/todos?_sort=id&_order=desc&subject_like=${searchText.value}&_page=${page}&_limit=${limit}`
    );
    totalTodos.value = res.headers["x-total-count"];
    todos.value = res.data;
  } catch (err) {
    console.log(err);
    triggerToast("Something went wrong", "danger");
  }
};

getTodos();

// const addTodo = async (todo) => {
//   error.value = "";
//   try {
//     await axios.post("http://localhost:3000/todos", {
//       subject: todo.subject,
//       completed: todo.completed,
//     });
//     getTodos();
//   } catch (err) {
//     console.log(err);
//     triggerToast("Something went wrong", "danger");
//   }
// };

const toggleTodo = async (index, checked) => {
  error.value = "";
  try {
    const id = todos.value[index].id;
    await axios.patch(`http://localhost:3000/todos/${id}`, {
      completed: checked,
    });
    getTodos();
  } catch (err) {
    console.log(err);
    triggerToast("Something went wrong", "danger");
  }
};

const deleteTodo = async (index) => {
  error.value = "";
  try {
    const id = todos.value[index].id;
    await axios.delete(`http://localhost:3000/todos/${id}`);
    getTodos();
  } catch (err) {
    console.log(err);
    triggerToast("Something went wrong", "danger");
  }
};

const createTodo = () => {
  router.push({ name: "TodoCreate" });
};
</script>

<style></style>
