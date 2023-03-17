<template>
  <div
    class="card mt-2"
    v-for="(todoList, index) in props.todos"
    :key="todoList.id"
  >
    <div
      class="card-body p-2 d-flex align-items-center"
      @click="writeTodo(todoList.id)"
      style="cursor: pointer"
    >
      <div class="form-check flex-grow-1">
        <input
          type="checkbox"
          class="form-check-input"
          :checked="todoList.completed"
          @change.stop="toggleTodo(index, $event)"
          @click.stop
        />
        <label
          class="form-check-label"
          :class="{ completedTodo: todoList.completed }"
          >{{ todoList.subject }}</label
        >
      </div>
      <div>
        <button class="btn btn-danger btn-sm" @click.stop="deleteTodo(index)">
          Delete
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";
import { useRouter } from "vue-router";

const props = defineProps({ todos: { type: Array, required: true } });
const emits = defineEmits(["toggle-todo", "delete-todo"]);
const router = useRouter();

const toggleTodo = (index, event) => {
  emits("toggle-todo", index, event.target.checked);
};

const deleteTodo = (index) => {
  emits("delete-todo", index);
};

const writeTodo = (todoId) => {
  // 이렇게 사용 할 수 있지만, url이 변경되면 이렇게 쓴 모든 코드를 수정해야한다.
  // router.push(`/todos/${todoId}`);

  // 이렇게 작성하면 name으로 통일되어 있기 때문에 router의 index.js에서만 수정해주면 된다.
  router.push({ name: "Todo", params: { id: todoId } });
};
</script>

<style>
.completedTodo {
  text-decoration: line-through;
  color: gray;
}
</style>
