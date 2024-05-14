<template>
  <div :class="{ 'task-pane-open': isOpen }" class="task-pane">
    <div class="header">
      <h3>Deal Description</h3>
      <button class="close-button primary-button" @click="closeTaskPane">X</button>
    </div>

    <div class="content">
      <h4><b>Name:</b> {{ data.deal_name }}</h4>
      <h4><b>Bloomberg: </b>{{ data.bloomberg_id }}</h4>
    </div>
  </div>
</template>

<script>
import { ref, onUpdated } from 'vue'

export default {
  props: {
    data: {
      type: Object,
      required: true
    },
    isOpenFormOutside: {
      type: Boolean,
      required: false
    }
  },
  setup(props, { emit }) {
    const isOpen = ref(false)

    const openTaskPane = () => {
      isOpen.value = true
      emit('isOpenFormOutsideEvent', isOpen.value)
    }

    const closeTaskPane = () => {
      isOpen.value = false
      emit('isOpenFormOutsideEvent', isOpen.value)
    }

    onUpdated(() => {
      if (props.isOpenFormOutside) {
        openTaskPane()
      } else {
        closeTaskPane()
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
h3 {
  font-weight: 700;
}
.header {
  display: flex;
  align-items: center;
  place-content: space-between;
}
.task-pane {
  width: 300px;
  position: fixed;
  top: 0;
  right: -300px;
  transition: right 0.3s ease;
  height: 100%;
  background: white;
  padding: 32px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);

  &.task-pane-open {
    right: 0;
  }
}
</style>
