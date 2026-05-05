<template>
  <div class="container">

    <!-- HEADER -->
    <div class="header">
      <h1>Employee Management</h1>
      <p>Create · Read · Update · Delete</p>

      <div class="cards">
        <div class="card">
          <h4>EMPLOYEES</h4>
          <p>{{ employees.length }}</p>
        </div>

        <div class="card">
          <h4>DEPARTMENTS</h4>
          <p>{{ uniqueDepartments }}</p>
        </div>

        <div class="card">
          <h4>AVG SALARY</h4>
          <p>₹{{ avgSalary }}</p>
        </div>

        <div class="card">
          <h4>LAST UPDATED</h4>
          <p>{{ today }}</p>
        </div>
      </div>
    </div>

    <div class="main">

      <!-- FORM -->
      <div class="form-box">
        <h2>Add Employee</h2>

        <form @submit.prevent="addEmployee">
          <input v-model="name" placeholder="Name" />
          <input v-model="designation" placeholder="Designation" />
          <input v-model="department" placeholder="Department" />
          <input v-model="salary" placeholder="Salary" type="number" />
          <button>{{ editIndex !== null ? "Update" : "Add Employee" }}</button>
        </form>
      </div>

      <!-- TABLE -->
      <div class="table-box">
        <h2>Employees</h2>

        <table>
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
            <tr v-for="(emp, index) in employees" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ emp.name }}</td>
              <td>{{ emp.designation }}</td>
              <td>{{ emp.department }}</td>
              <td>₹{{ emp.salary }}</td>
              <td>
                <button class="edit" @click="editEmployee(index)">Edit</button>
                <button class="delete" @click="deleteEmployee(index)">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>

      </div>

    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      name: "",
      designation: "",
      department: "",
      salary: "",
      employees: [],
      editIndex: null
    };
  },

  methods: {
    addEmployee() {
      if (this.editIndex !== null) {
        this.employees[this.editIndex] = {
          name: this.name,
          designation: this.designation,
          department: this.department,
          salary: this.salary
        };
        this.editIndex = null;
      } else {
        this.employees.push({
          name: this.name,
          designation: this.designation,
          department: this.department,
          salary: this.salary
        });
      }

      this.name = "";
      this.designation = "";
      this.department = "";
      this.salary = "";
    },

    deleteEmployee(index) {
      this.employees.splice(index, 1);
    },

    editEmployee(index) {
      const emp = this.employees[index];
      this.name = emp.name;
      this.designation = emp.designation;
      this.department = emp.department;
      this.salary = emp.salary;
      this.editIndex = index;
    }
  },

  computed: {
    uniqueDepartments() {
      const set = new Set(this.employees.map(e => e.department));
      return set.size;
    },
    avgSalary() {
      if (this.employees.length === 0) return 0;
      const total = this.employees.reduce((sum, e) => sum + Number(e.salary), 0);
      return Math.floor(total / this.employees.length);
    },
    today() {
      return new Date().toLocaleDateString();
    }
  }
};
</script>