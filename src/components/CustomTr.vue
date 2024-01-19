<template>
    <tr>
      <slot :item="item" v-bind="slotsProps"></slot>
    </tr>
  </template>
  
  <script setup>
  import { defineProps } from 'vue';
  
  const { columns, item } = defineProps(['columns', 'item']);
  const slotsProps = {
    columns,
    item,
  };
  </script>