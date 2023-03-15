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
// 이벤트명을 명시해주는 것.
const emits = defineEmits(["add-todo"]);

const onSubmit = () => {
  if (todo.value === "") {
    hasError.value = true;
  } else {
    // 명시한 이벤트가 발생했을 때, 두번째 인자에 있는 데이터를 보내준다.
    // 새로운 이벤트를 정의하고, 이벤트를 실행했을 때 자식 컴포넌트에 있는 데이터를 올려줄 수 있다.
    emits("add-todo", {
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
