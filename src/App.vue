<template>
  <div v-show="toggle">true</div>
  <div v-show="!toggle">false</div>
  <button @click="onToggle">Toggle</button>
  <div class="container">
    <h2>To-Do List</h2>
    <form class="d-flex" @submit.prevent="onSubmit">
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
    </form>
    <div class="card mt-2" v-for="todo in todos" :key="todo.id">
      <div class="card-body p-2">{{ todo.subject }}</div>
    </div>
  </div>
</template>

<script>
import { reactive, ref } from "vue";
export default {
  setup() {
    const toggle = ref(false);
    const todo = ref("");
    const todos = reactive([
      { id: 1, subject: "휴대폰 사기" },
      { id: 2, subject: "장보기" },
    ]);

    const onSubmit = () => {
      todos.push({
        id: Date.now(),
        subject: todo.value,
      });
      todo.value = "";
    };

    const onToggle = () => {
      toggle.value = !toggle.value;
    };

    return { todo, todos, toggle, onSubmit, onToggle };
  },
};
</script>

<style>
.name {
  color: red;
}
</style>
