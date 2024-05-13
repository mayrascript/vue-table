<template>
  <div id="app">
    <DealGrid />
    <TaskPane />
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
    const selectedDeals = ref([]);
    provide('selectedDeals', selectedDeals);

    return {};
  }
};
</script>
