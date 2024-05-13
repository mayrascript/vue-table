<template>
  <div id="app">
    <DealGrid :data="sharedData" @updateData="updateData"/>
    <TaskPane :data="sharedData" :isOpenFormOutside="isOpenFormOutside" @isOpenFormOutsideEvent="isOpenFormOutsideEvent"/>
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
    const isOpenFormOutside = ref(false);


    const updateData = (newData) => {
      const parsedData = JSON.parse(JSON.stringify(newData.value));
      const singleData =  parsedData?.[0];

      if(!singleData) {
        isOpenFormOutside.value = false;
      }else {
        const isMultipleSelection = parsedData?.length > 1;
        isOpenFormOutside.value = !isMultipleSelection;
        sharedData.value = singleData;
      }
    };
    const isOpenFormOutsideEvent = (isOpen) => {
      isOpenFormOutside.value = isOpen;
    }

    return {
      sharedData,
      updateData,
      isOpenFormOutsideEvent,
      isOpenFormOutside
    };
  }
};
</script>
