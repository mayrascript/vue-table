<template>
  <div id="app">
    <DealGrid />
  </div>
</template>

<script>
import DealGrid from './components/DealGrid.vue';
import { provide, ref } from 'vue';

export default {
  components: {
    DealGrid,
  },
  setup() {
    const selectedDeals = ref([]);
    provide('selectedDeals', selectedDeals);

    return {};
  }
};
</script>
