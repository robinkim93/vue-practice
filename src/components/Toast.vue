<template>
  <div
    class="alert toast-box"
    role="alert"
    :class="`alert-${props.toastAlertType}`"
  >
    {{ props.toastMessage }}
  </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  toastMessage: { type: String, required: true },
  toastAlertType: { type: String, default: "success" },
});
</script>

<style>
.toast-box {
  position: fixed;
  top: 10px;
  right: 10px;
}
</style>
