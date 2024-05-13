<template>
  <div :class="{ 'task-pane-open': isOpen }" class="task-pane">
    <div class="content">
      <p>ID: {{ data?.id }}</p>
      <p>Issues Name: {{ data?.issuer_name }}</p>
    </div>
    <button @click="closeTaskPane" class="close-button">Close</button>
  </div>
</template>

<script>
import { ref, defineProps, onUpdated } from 'vue'

export default {
  props: {
    data: {
      type: Object,
      required: true
    }
  },
  setup(props) {

    const isOpen = ref(true);

    const openTaskPane = () => {
      isOpen.value = true;
    };

    const closeTaskPane = () => {
      isOpen.value = false;
    };

    return {
      props
    }
  }
}



</script>

<style lang="scss">
.task-pane {
  width: 300px;
  position: fixed;
  top: 0;
  right: 0;
  transition: right 0.3s ease;
  outline: 1px solid red;
  height: 100%;
  background: white;
  padding: 16px;

  &.task-pane-open {
    right: 300px;
  }

  .content {
  }

  .close-button {
    }
}
</style>
