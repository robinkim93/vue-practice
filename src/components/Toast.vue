<template>
  <div class="alert alert-success toast-box" role="alert">
    {{ props.toastMessage }}
  </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({ toastMessage: { type: String, required: true } });
</script>

<style>
.toast-box {
  position: fixed;
  top: 10px;
  right: 10px;
}
</style>
