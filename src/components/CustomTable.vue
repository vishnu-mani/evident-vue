<template>
    <table cellpadding="0" cellspacing="0">
      <thead>
        <tr>
          <th v-for="column in columns" @click="sortColumn(column)">
            {{ column.label }}
          </th>
        </tr>
      </thead>
      <tbody>
        <CustomTr v-for="(item, index) in data" :key="index" :item="item" :columns="columns">
          <CustomTd
            v-for="column in columns"
            :key="column.label"
            :column="column"
            :value="item[column.key]">
              <img v-if="column.key === 'picture'" :src="item[column.key]">
              <p v-else-if="column.key === 'location'">{{ item[column.key].city }}, {{ item[column.key].state }}, {{ item[column.key].country }}</p>
              <p v-else>{{ item[column.key] }}</p>
          </CustomTd>
        </CustomTr>
      </tbody>
    </table>
  </template>
  
<script setup>
  import { defineProps, defineEmits } from 'vue';
  import CustomTr from './CustomTr.vue';
  import CustomTd from './CustomTd.vue';
  
  const props = defineProps(['data', 'columns']);
  const emits = defineEmits(['sort']);
  
  const sortColumn = (column) => {
    if ( column.sortable ) {
      column.sortOrder = column.sortOrder === 'asc' ? 'desc' : 'asc';
      emits('sort', column);
    }
  };
</script>

<style scoped>
img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 1px solid #ddd;
  margin: auto;
}
table {
  border-collapse: collapse;
  width: 100%;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 15px;
}
table th {
  background-color: #ccc;
  border: 1px solid #ddd;
  padding: 10px 15px;
  cursor: pointer;
}

</style>