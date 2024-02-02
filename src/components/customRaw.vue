<template>
  <div>
    <p v-for="emp in props.employees" :key="emp">
      <slot :employe="emp" :index="1"></slot>
    </p>
  </div>
</template>


<script setup>
import { defineProps } from "vue";

const props = defineProps({ employees: Array });
</script>
