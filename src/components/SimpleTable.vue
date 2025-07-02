<template>
  <VDataTable :data="users" :headers="tableHeaders">
    <template #body="{ row }">
      <td>{{ row.name }}</td>
      <td>{{ row.age }}</td>
      <td>{{ row.address.state }}</td>
      <td>{{ row.registered }}</td>
    </template>
  </VDataTable>
</template>

<script setup>
import VDataTable from './VDataTable.vue';

const users = [
  { guid: 1, name: 'Alice', age: 25, address: { state: 'CA' }, registered: '2024-06-01' },
  { guid: 2, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-02' },
  { guid: 3, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-03' },
  { guid: 4, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-04' },
  { guid: 5, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-05' },
  { guid: 6, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-06' },
  { guid: 7, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-07' },
  { guid: 8, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-08' },
  { guid: 9, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-09' },
  { guid: 10, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-10' },
  { guid: 11, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-11' },
  { guid: 12, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-12' },
  { guid: 13, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-13' },
  { guid: 14, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-14' },
  { guid: 15, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-15' },
  { guid: 16, name: 'Bob', age: 30, address: { state: 'TX' }, registered: '2024-05-16' },
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
  { label: 'Name', sortKey: 'name', customSort: nameLengthSort, defaultSort: 'desc' },
  { label: 'Age', sortKey: 'age' },
  { label: 'State', sortKey: 'address.state' },
  { label: 'Registered', customSort: dateSort },
];
</script>

