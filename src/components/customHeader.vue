<template>
  <div class="table-head">
    <div v-for="hed in props.fields" :key="hed" class="head-field">
      {{ hed }}
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({ fields: Array });
</script>
<style scoped>
.table-head {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
.head-field {
  margin: 10px;
  padding: 10px;
}
</style>
