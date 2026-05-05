<template>
  <form @submit.prevent="handleSubmit">
    <input v-model="name" placeholder="Name" />
    <input v-model="role" placeholder="Role" />
    <button type="submit">Add</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      role: ""
    };
  },
  methods: {
    handleSubmit() {
      this.$emit("add-employee", {
        name: this.name,
        role: this.role
      });
      this.name = "";
      this.role = "";
    }
  }
};
</script>