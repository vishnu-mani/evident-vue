<template>
    <td>
      <slot :column="column" :value="value"></slot>
    </td>
</template>
  
<script setup>
  import { defineProps } from 'vue';
  
  const props = defineProps(['column', 'value']);
</script>

<style scoped>
  td {
    border: 1px solid #ddd;
    padding: 3px 10px;
    /* text-align: center; */
  }
</style>