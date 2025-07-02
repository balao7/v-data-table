<template>
  <div class="v-data-table">
    <table>
      <thead>
        <tr>
          <th
            v-for="(header, index) in headers"
            :key="index"
            @click="handleSort(header)"
            :class="{ sortable: header.sortKey || header.customSort }"
          >
            {{ header.label }}
            <span v-if="header.sortKey || header.customSort">
              {{ getSortIcon(header) }}
            </span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="row in sortedData" :key="row.id || row.guid">
          <slot name="body" :row="row" />
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { computed, ref, watch } from 'vue';

const props = defineProps({
  data: { type: Array, required: true },
  headers: { type: Array, required: true },
});

const emit = defineEmits(['sort']);

const sortKey = ref('');
const sortOrder = ref('asc'); // 'asc' or 'desc'

const handleSort = (header) => {
  if (!(header.sortKey || header.customSort)) return;

  if (sortKey.value === (header.sortKey || header.label)) {
    sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc';
  } else {
    sortKey.value = header.sortKey || header.label;
    sortOrder.value = header.defaultSort || 'none';
  }

  emit('sort', { key: sortKey.value, order: sortOrder.value });
};

const sortedData = computed(() => {
  if (!sortKey.value) return props.data;

  const header = props.headers.find(
    (h) => h.sortKey === sortKey.value || h.label === sortKey.value
  );

  if (!header) return props.data;

  const sorted = [...props.data];

  if (header.customSort) {
    sorted.sort((a, b) => header.customSort(a, b, sortOrder.value));
  } else {
    sorted.sort((a, b) => {
      const valA = resolveValue(a, header.sortKey);
      const valB = resolveValue(b, header.sortKey);

      if (valA < valB) return sortOrder.value === 'asc' ? -1 : 1;
      if (valA > valB) return sortOrder.value === 'asc' ? 1 : -1;
      return 0;
    });
  }

  return sorted;
});

const resolveValue = (obj, path) => {
  return path.split('.').reduce((acc, part) => acc?.[part], obj);
};

const getSortIcon = (header) => {
  if ((header.sortKey || header.customSort) && sortKey.value === (header.sortKey || header.label)) {
    return sortOrder.value === 'asc' ? '▲' : '▼';
  }
  return '↕';
};
</script>

<style scoped>
.sortable {
  cursor: pointer;
  user-select: none;
}
</style>
