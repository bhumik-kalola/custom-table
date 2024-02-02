<template>
  <table style="border: 2px solid black">
    <slot name="thead">
      <thead>
        <slot name="headRow" class="custom-row">
          <tr>
            <template v-for="field in props.fields">
              <slot
                :name="`head(${field.key})`"
                :field="field"
                :value="field.label"
              >
                <th :key="field.key" @click="sortBy(field.key)">
                  {{ field.label }}
                  <span v-if="sortField === field.key">
                    {{ sortOrder === "asc" ? "▲" : "▼" }}
                  </span>
                </th>
              </slot>
            </template>
          </tr>
        </slot>
      </thead>
    </slot>
    <tbody v-if="rows.length">
      <template v-for="row in sortedRows">
        <slot :name="`row(${getKey(row)})`" :row="row">
          <tr :key="getKey(row)">
            <template v-for="objectKey in visibleFieldKeys">
              <slot
                :name="`cell(${objectKey})`"
                :value="row[objectKey]"
                :row="row"
              >
                <td :key="objectKey">
                  {{ row[objectKey] }}
                </td>
              </slot>
            </template>
          </tr>
        </slot>
      </template>
    </tbody>
    <tbody v-else>
      <slot name="no-data-row">
        <tr>
          <slot name="no-data-cell">
            <td :colspan="fields.length">
              {{ props.emptyMessage }}
            </td>
          </slot>
        </tr>
      </slot>
    </tbody>
  </table>
</template>

<script setup>
import { defineProps, computed, ref } from "vue";

const props = defineProps(["fields", "rows", "emptyMessage"]);

const getKey = (row) => (row?.key || row?.id) ?? JSON.stringify(row);

const visibleFieldKeys = computed(() => props.fields.map((field) => field.key));

const sortField = ref(null);
const sortOrder = ref("desc");

const sortedRows = computed(() => {
  if (!sortField.value) return props.rows;
  return props.rows.slice().sort((a, b) => {
    const aValue = a[sortField.value];
    const bValue = b[sortField.value];
    if (aValue < bValue) return sortOrder.value === "asc" ? -1 : 1;
    if (aValue > bValue) return sortOrder.value === "asc" ? 1 : -1;
    return 0;
  });
});

const sortBy = (field) => {
  if (sortField.value === field) {
    sortOrder.value = sortOrder.value === "asc" ? "desc" : "asc";
  } else {
    sortField.value = field;
    sortOrder.value = "asc";
  }
};
</script>

<style scoped>
table,
th,
td {
  border: 1px solid black;
}
</style>
