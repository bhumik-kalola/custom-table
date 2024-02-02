<template>
  <customTable :fields="fields" :rows="rows">
  </customTable>
</template>

<script setup>
import { ref, onMounted } from "vue"; 
import customTable from "@/components/customTable.vue"; 
import axios from "axios";

const fields = [
  { key: "slno", label: "slno" },
  { key: "name", label: "name" },
  { key: "DOB", label: "dob" },
  { key: "Email", label: "email" },
  { key: "Location", label: "location" },
  { key: "Phone", label: "phone" },
  { key: "Picture", label: "picture" },
];

const rows = ref([]);
const getData = () => {
  axios.get("https://randomuser.me/api/?results=50").then((res) => {
    console.log("data", res.data.results);
    rows.value = res.data.results.map((a) => ({
      slno: a.id.value,
      name: a.name.first + a.name.last,
      DOB: a.dob.date.split("T")[0],
      Email: a.email,
      Location: `${a.location.street.number},${a.location.street.name},${a.location.city},${a.location.state},${a.location.country},${a.location.postcode}`,
      Phone: a.phone,
      Picture: a.picture.medium,
    }));
  });
};

onMounted(() => {
  getData(); 
});
</script>
