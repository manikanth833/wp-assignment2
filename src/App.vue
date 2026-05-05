<template>
  <div class="container mt-4">

    <h2 class="text-center mb-4">Employee Management</h2>

    <!-- FORM -->
    <div class="card shadow p-4 mb-4">
      <h4 class="mb-3">Add Employee</h4>

      <form @submit.prevent="addEmployee" class="row g-3">
        <div class="col-md-3">
          <input v-model="name" class="form-control" placeholder="Name" required />
        </div>

        <div class="col-md-3">
          <input v-model="designation" class="form-control" placeholder="Designation" required />
        </div>

        <div class="col-md-3">
          <input v-model="department" class="form-control" placeholder="Department" required />
        </div>

        <div class="col-md-2">
          <input v-model="salary" type="number" class="form-control" placeholder="Salary" required />
        </div>

        <div class="col-md-1">
          <button class="btn btn-primary w-100">
            {{ editId ? "Update" : "Add" }}
          </button>
        </div>
      </form>
    </div>

    <!-- TABLE -->
    <div class="card shadow p-3">
      <h4 class="mb-3">Employees</h4>

      <table class="table table-bordered table-hover">
        <thead class="table-dark">
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
              <button class="btn btn-warning btn-sm me-2" @click="editEmployee(emp)">Edit</button>
              <button class="btn btn-danger btn-sm" @click="deleteEmployee(emp.id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

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