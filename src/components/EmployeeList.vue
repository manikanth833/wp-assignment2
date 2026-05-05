<template>
  <div>
    <h2>Employee List</h2>
    <div v-for="(emp, index) in employees" :key="index">
      <p>{{ emp.name }} - {{ emp.role }}</p>
      <button @click="$emit('delete-employee', index)">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["employees"]
};
</script>