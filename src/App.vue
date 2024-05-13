<template>
  <div id="app">
    <DealGrid :data="sharedData" @updateData="updateData"/>
    <TaskPane :data="sharedData" />
  </div>
</template>

<script>
import DealGrid from './components/DealGrid.vue';
import { provide, ref } from 'vue';
import TaskPane from '@/components/TaskPane.vue'

export default {
  components: {
    TaskPane,
    DealGrid,
  },
  setup() {
    const sharedData = ref([]);

    const updateData = (newData) => {
      console.log("PARENT COMPONENT: updateData", newData)
      sharedData.value = newData;
    };

    return {
      sharedData,
      updateData
    };
  }
};
</script>
