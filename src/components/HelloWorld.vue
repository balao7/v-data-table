<script setup>
import { ref } from 'vue'
import SimpleTable from './SimpleTable.vue'

defineProps({
  msg: String,
})

const count = ref(0)
</script>

<template>
  <h2>{{ msg }}</h2>
  <div style="display: block;">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <div class="table-content">
    <SimpleTable />
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
.table-content {
  padding: 2rem, 2rem, 0;
}
</style>
