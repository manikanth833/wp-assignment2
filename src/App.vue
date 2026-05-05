<template>
  <div class="container">
    <h1>Employee Management</h1>

    <!-- FORM -->
    <form @submit.prevent="addEmployee">
      <input v-model="name" placeholder="Name" required />
      <input v-model="designation" placeholder="Designation" required />
      <input v-model="department" placeholder="Department" required />
      <input v-model="salary" type="number" placeholder="Salary" required />
      <button>
        {{ editId ? "Update" : "Add Employee" }}
      </button>
    </form>

    <!-- TABLE -->
    <table border="1" width="100%" style="margin-top:20px">
      <thead>
        <tr>
          <th>#</th>
          <th>Name</th>
          <th>Designation</th>
          <th>Department</th>
          <th>Salary</th>
          <th>Actions</th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="(emp, index) in employees" :key="emp.id">
          <td>{{ index + 1 }}</td>
          <td>{{ emp.name }}</td>
          <td>{{ emp.designation }}</td>
          <td>{{ emp.department }}</td>
          <td>₹{{ emp.salary }}</td>
          <td>
            <button @click="editEmployee(emp)">Edit</button>
            <button @click="deleteEmployee(emp.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios"

const API = "https://69f996a5c509a40d3aa2be5b.mockapi.io/emp/employees"

export default {
  data() {
    return {
      name: "",
      designation: "",
      department: "",
      salary: "",
      editId: null,
      employees: []
    }
  },

  mounted() {
    this.getEmployees()
  },

  methods: {
    async getEmployees() {
      try {
        const res = await axios.get(API)
        this.employees = res.data || []
      } catch (e) {
        console.log("API error", e)
        // fallback so UI never blank
        this.employees = [
          {
            id: 1,
            name: "Demo",
            designation: "Developer",
            department: "IT",
            salary: 50000
          }
        ]
      }
    },

    async addEmployee() {
      const emp = {
        name: this.name,
        designation: this.designation,
        department: this.department,
        salary: this.salary
      }

      try {
        if (this.editId) {
          await axios.put(`${API}/${this.editId}`, emp)
          this.editId = null
        } else {
          await axios.post(API, emp)
        }
        this.getEmployees()
      } catch (e) {
        console.log("Save error", e)
      }

      this.name = ""
      this.designation = ""
      this.department = ""
      this.salary = ""
    },

    editEmployee(emp) {
      this.name = emp.name
      this.designation = emp.designation
      this.department = emp.department
      this.salary = emp.salary
      this.editId = emp.id
    },

    async deleteEmployee(id) {
      try {
        await axios.delete(`${API}/${id}`)
        this.getEmployees()
      } catch (e) {
        console.log("Delete error", e)
      }
    }
  }
}
</script>