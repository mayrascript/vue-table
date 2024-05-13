<template>
  <div :class="{ 'task-pane-open': isOpen }" class="task-pane">
    <div class="content">
    </div>
    <button @click="closeTaskPane" class="close-button">Close</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const isOpen = ref(true);

const openTaskPane = () => {
  isOpen.value = true;
};

const closeTaskPane = () => {
  isOpen.value = false;
};
</script>

<style lang="scss">
.task-pane {
  width: 300px;
  position: fixed;
  top: 0;
  right: -300px;
  transition: right 0.3s ease;
  outline: 1px solid red;
  height: 100%;
  background: white;
  padding: 16px;

  &.task-pane-open {
    right: 0;
  }

  .content {
  }

  .close-button {
    }
}
</style>
