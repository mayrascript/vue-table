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
    },
    isOpenFormOutside: {
      type: Boolean,
      required: false
    },
  },
  setup(props, {emit}) {

    const isOpen = ref(false);

    const openTaskPane = () => {
      isOpen.value = true;
      emit('isOpenFormOutsideEvent', isOpen.value);
    };

    const closeTaskPane = () => {
      isOpen.value = false;
      emit('isOpenFormOutsideEvent', isOpen.value);
    };

    onUpdated(() => {
      if(props.isOpenFormOutside) {
        openTaskPane()
      }
    })

    return {
      props,
      openTaskPane,
      closeTaskPane,
      isOpen
    }
  }
}



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
