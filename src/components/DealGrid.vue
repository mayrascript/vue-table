<template>
  <div class="deal-grid">
    <div class="search-bar">
      <input type="text" placeholder="Search Deals" v-model="searchQuery" />
    </div>
    <table>
      <thead>
      <tr>
        <th v-for="column in columns" :key="column.key"
            @click="sortColumn(column.key)">
          {{ column.label }}
          <span v-if="sortKey === column.key">
              {{ sortOrder === 'asc' ? '▲' : '▼' }}
            </span>
        </th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="deal in filteredDeals" :key="deal.id"
          @click="selectDeal(deal); updateParent(deal)"
          :class="{ selected: selectedDeals.includes(deal) }">
        <td v-for="column in columns" :key="column.key">
            <span v-if="column.type === 'string[]'">
              {{ deal[column.key].join(', ') }}
            </span>
          <span v-else>{{ deal[column.key] }}</span>
        </td>
      </tr>
      </tbody>
    </table>

    <button @click="exportToCSV">Export to CSV</button>
  </div>
</template>

<script>

import { ref, computed, onMounted, defineProps, defineEmits } from 'vue';
import axios from 'axios';

export default {
  setup(_, { emit }) {
    const props = defineProps(['data'])
    const inputData = ref('');

    const updateParent = (deal) => {
      inputData.value = deal;
      console.log("COMPONENT A: updateParent", deal)

      emit('updateData', inputData.value);
    };

    const columns = ref(   [{ key: 'id', type: 'int', label: 'ID' },
      { key: 'issuer_name', type: 'string', label: 'Issuer' },
      { key: 'deal_name', type: 'string', label: 'Deal' },
      { key: 'bloomberg_id', type: 'string', label: 'Bloomberg ID' },
      { key: 'total', type: 'decimal', label: 'Total' },
      { key: 'industry', type: 'string', label: 'Industry' },
      { key: 'status', type: 'string', label: 'Status' },
      { key: 'analysts', type: 'string[]', label: 'Analysts' },
      { key: 'doc_count', type: 'int', label: 'Docs' },
      { key: 'custom_deal_identifiers', type: 'string[]', label: 'Identifiers' }]);

    const deals = ref([]);
    const searchQuery = ref('');
    const sortKey = ref(null);
    const sortOrder = ref('asc');
    const selectedDeals = ref([]);

    onMounted(async () => {
      try {
        const response = await axios.get('../src/data/deals.json');
        deals.value = response.data;
      } catch (error) {
        console.error('Error fetching deals:', error);
      }
    });

    const filteredDeals = computed(() => {
      let filtered = deals.value.filter(deal => {
        return Object.values(deal).some(value => {
          if (typeof value === 'string') {
            return value.toLowerCase().includes(searchQuery.value.toLowerCase());
          } else if (Array.isArray(value)) {
            return value.some(item =>
              item.toLowerCase().includes(searchQuery.value.toLowerCase())
            );
          }
          return false;
        });
      });

      if (sortKey.value) {
        filtered.sort((a, b) => {
          const aVal = a[sortKey.value];
          const bVal = b[sortKey.value];
          if (sortOrder.value === 'asc') {
            return aVal > bVal ? 1 : aVal < bVal ? -1 : 0;
          } else {
            return aVal < bVal ? 1 : aVal > bVal ? -1 : 0;
          }
        });
      }

      return filtered;
    });

    const sortColumn = (key) => {
      if (sortKey.value === key) {
        sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc';
      } else {
        sortKey.value = key;
        sortOrder.value = 'asc';
      }
    };

    const selectDeal = (deal) => {
      if (selectedDeals.value.includes(deal)) {
        selectedDeals.value = selectedDeals.value.filter(d => d !== deal);
      } else {
        selectedDeals.value = [deal];
      }

/*      console.log('selectedDeals',JSON.parse( JSON.stringify(selectedDeals.value)))
      updateParent(selectedDeals.value)*/
    };

    const exportToCSV = () => {
      let csvContent = "data:text/csv;charset=utf-8,";
      csvContent += columns.value.map(column => column.label).join(',') + '\n';
      filteredDeals.value.forEach(deal => {
        csvContent += columns.value.map(column => {
          const value = deal[column.key];
          if (Array.isArray(value)) {
            return value.join(';');
          }
          return value;
        }).join(',') + '\n';
      });

      const encodedUri = encodeURI(csvContent);
      const link = document.createElement("a");
      link.setAttribute("href", encodedUri);
      link.setAttribute("download", "deals.csv");
      document.body.appendChild(link);

      link.click();
      document.body.removeChild(link);
    };

    return {
      columns,
      filteredDeals,
      searchQuery,
      sortKey,
      sortOrder,
      selectedDeals,
      sortColumn,
      selectDeal,
      exportToCSV,
      inputData,
      updateParent
    };
  },
};
</script>

<style lang="scss">
.deal-grid {
  .search-bar {
    margin-bottom: 10px;
  }

  table {
    width: 100%;
    border-collapse: collapse;

    th, td {
      padding: 8px;
      border: 1px solid #ddd;
      text-align: left;
    }

    th {
      cursor: pointer;
    }

    .selected {
      background-color: #f0f0f0;
    }
  }
}
</style>
