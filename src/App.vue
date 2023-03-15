<template>
  <div class="container">
    <h2>To-Do List</h2>
    <input
      class="form-control"
      type="text"
      v-model="searchText"
      placeholder="Search your Todo"
    />
    <hr />
    <!-- 명시한 이벤트가 발생했을 때 실행 될 로직을 추가해준다. -->
    <TodoSimpleForm @add-todo="addTodo" />
    <div class="errorMessage">{{ error }}</div>
    <div v-if="!filteredTodos.length" class="mt-2">표시할 Todo가 없습니다</div>
    <TodoList
      :todos="filteredTodos"
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item"><a class="page-link" href="#">Previous</a></li>
        <li class="page-item"><a class="page-link" href="#">1</a></li>
        <li class="page-item"><a class="page-link" href="#">2</a></li>
        <li class="page-item"><a class="page-link" href="#">3</a></li>
        <li class="page-item"><a class="page-link" href="#">Next</a></li>
      </ul>
    </nav>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";
import axios from "axios";
import TodoSimpleForm from "./components/TodoSimpleForm.vue";
import TodoList from "./components/TodoList.vue";

const todos = ref([]);
const searchText = ref("");
const error = ref("");

// emit으로 올라온 이벤트가 발생했을 때 실행 될 함수를 정의해줬다
// 이 때, 인자를 하나 받아오게 되면 자식 컴포넌트에서 올려준 데이터를 받을 수 있다.

const getTodos = async () => {
  try {
    const res = await axios.get("http://localhost:3000/todos?_page=1&_limit=5");
    todos.value = res.data;
  } catch (err) {
    console.log(err);
    error.value = "Something Error";
  }
};

getTodos();

const addTodo = async (todo) => {
  error.value = "";
  try {
    await axios.post("http://localhost:3000/todos", {
      subject: todo.subject,
      completed: todo.completed,
    });
    getTodos();
  } catch (err) {
    console.log(err);
    error.value = "Something Error";
  }
};

const toggleTodo = async (index) => {
  error.value = "";
  try {
    const id = todos.value[index].id;
    await axios.patch(`http://localhost:3000/todos/${id}`, {
      completed: !todos.value[index].completed,
    });
    getTodos();
  } catch (err) {
    console.log(err);
    error.value = "Something Error";
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
    error.value = "Something Error";
  }
};

const filteredTodos = computed(() => {
  if (searchText.value) {
    return todos.value.filter((todo) =>
      todo.subject.includes(searchText.value)
    );
  }
  return todos.value;
});
</script>

<style>
.errorMessage {
  color: red;
}
</style>
