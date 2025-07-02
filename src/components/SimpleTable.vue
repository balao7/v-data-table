<template>
  <div>
  <VDataTable :data="sortedUsers" :headers="tableHeaders" @sort="updateSort">
    <template #body="{ row }">
      <td>{{ row.name }}</td>
      <td>{{ row.lastname }}</td>
      <td>{{ row.age }}</td>
      <td>{{ row.address.state }}</td>
      <td>{{ row.registered }}</td>
    </template>
  </VDataTable>

  <div class="info-bar">
      Showing {{ visibleCount }} of {{ users.length }}
    </div>

    <button v-if="visibleCount < users.length" @click="showMore">
      Show More
    </button>
  </div>
</template>

<script setup>
import { computed, ref, watch } from 'vue';
import VDataTable from './VDataTable.vue';

const users = [
  { guid: 1, name: 'Alice', lastname: 'Bob', age: 25, address: { state: 'CA' }, registered: '2024-06-01' },
  { guid: 2, name: 'Alice', lastname: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-02' },
  { guid: 3, name: 'Alice', lastname: 'Bob', age: 10, address: { state: 'TX' }, registered: '2024-05-03' },
  { guid: 4, name: 'Alice', lastname: 'Bob', age: 20, address: { state: 'TX' }, registered: '2024-05-04' },
  { guid: 5, name: 'Alice', lastname: 'Bob', age: 40, address: { state: 'TX' }, registered: '2024-05-05' },
  { guid: 6, name: 'Alice', lastname: 'Bob', age: 50, address: { state: 'TX' }, registered: '2024-05-06' },
  { guid: 7, name: 'Alice', lastname: 'Bob', age: 10, address: { state: 'TX' }, registered: '2024-05-07' },
  { guid: 8, name: 'Alice', lastname: 'Bob', age: 15, address: { state: 'TX' }, registered: '2024-05-08' },
  { guid: 9, name: 'Alice', lastname: 'Bob', age: 35, address: { state: 'TX' }, registered: '2024-05-09' },
  { guid: 10, name: 'Alice', lastname: 'Bob', age: 45, address: { state: 'TX' }, registered: '2024-05-10' },
  { guid: 11, name: 'Alice', lastname: 'Bob', age: 55, address: { state: 'TX' }, registered: '2024-05-11' },
  { guid: 12, name: 'Alice', lastname: 'Bob', age: 60, address: { state: 'TX' }, registered: '2024-05-12' },
  { guid: 13, name: 'Alice', lastname: 'Bob', age: 65, address: { state: 'TX' }, registered: '2024-05-13' },
  { guid: 14, name: 'Alice', lastname: 'Bob', age: 70, address: { state: 'TX' }, registered: '2024-05-14' },
  { guid: 15, name: 'Alice', lastname: 'Bob', age: 75, address: { state: 'TX' }, registered: '2024-05-15' },
  { guid: 16, name: 'Alice', lastname: 'Bob', age: 80, address: { state: 'TX' }, registered: '2024-05-16' },
];

const dateSort = (a, b, order) => {
  const d1 = new Date(a.registered).getTime();
  const d2 = new Date(b.registered).getTime();
  return order === 'asc' ? d1 - d2 : d2 - d1;
};

const nameLengthSort = (a, b, order) => {
  const l1 = a.name.length;
  const l2 = b.name.length;
  return order === 'asc' ? l1 - l2 : l2 - l1;
};

const tableHeaders = [
  { label: 'Name', sortKey: 'name', customSort: nameLengthSort, defaultSort: 'asc' },
  { label: 'Last Name', sortKey: 'lastname' },
  { label: 'Age', sortKey: 'age' },
  { label: 'State', sortKey: 'address.state' },
  { label: 'Registered', customSort: dateSort },
];

const currentSort = ref('name');
const currentSortDir = ref('asc');
const visibleCount = ref(10); // Initial rows to show
const maxVisible = 10;
const dynamicPageSize = ref(0);

const updateSort = (key) => {
  if (currentSort.value === key) {
    currentSortDir.value = currentSortDir.value === 'asc' ? 'desc' : 'asc';
  } else {
    currentSort.value = key;
    currentSortDir.value = 'asc';
  }
};

const sortedUsers = computed(() => {
  return [...users].sort((a, b) => {
    const valA = resolveValue(a, currentSort.value);
    const valB = resolveValue(b, currentSort.value);
    const modifier = currentSortDir.value === 'asc' ? 1 : -1;
    if (valA < valB) return -1 * modifier;
    if (valA > valB) return 1 * modifier;
    return 0;
  }).filter((row, index) => {
    return index < visibleCount.value;
  })
});

const resolveValue = (obj, path) => path.split('.').reduce((acc, key) => acc?.[key], obj);

const showMore = () => {
  visibleCount.value += dynamicPageSize;
  if (visibleCount.value > users.length) {
    visibleCount.value = users.length;
  }
};

// Initial setup for visible count & dynamic page size
watch(
  () => users.length,
  (newLen) => {
    visibleCount.value = newLen <= maxVisible ? newLen : maxVisible;
  },
  { immediate: true }
);
</script>

<style scoped>
.info-bar {
  margin-top: 8px;
  margin-bottom: 8px;
}
</style>
