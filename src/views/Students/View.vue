<!-- @format -->
<script>
import axios from "axios";

export default {
  name: "students",
  data() {
    return {
      students: [],
    };
  },
  methods: {
    getStundets() {
      axios.get("http://localhost:8000/api/students").then((response) => {
        this.students = response.data;
        console.log(this.students);
      });
    },

    deleteStudent(studentId) {
      if (confirm("Are you sure, you want to delete this student?")) {
        axios
          .delete(`http://localhost:8000/api/students/${studentId}/delete`)
          .then((response) => {
            console.log(response.data.message);
            this.getStundets();
          });
      }
    },
  },
  mounted() {
    this.getStundets();
  },
};
</script>

<template>
  <div class="container">
    <div class="card">
      <div class="card-header">
        <h4>
          Student
          <RouterLink to="/students/create" class="btn btn-primary float-end"
            >Add Student</RouterLink
          >
        </h4>
      </div>
      <div class="card-body">
        <table class="table table-bordered">
          <thead>
            <tr>
              <th>ID</th>
              <th>Name</th>
              <th>Course</th>
              <th>Email</th>
              <th>Phone</th>
              <th>Created At</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody v-if="this.students.length > 0">
            <tr v-for="(student, index) in this.students" :key="index">
              <td>{{ student.id }}</td>
              <td>{{ student.name }}</td>
              <td>{{ student.course }}</td>
              <td>{{ student.email }}</td>
              <td>{{ student.phone }}</td>
              <td>{{ student.created_at }}</td>
              <td>
                <RouterLink
                  :to="{ path: '/students/' + student.id + '/edit' }"
                  class="btn btn-success"
                  >Edit</RouterLink
                >
                <button
                  type="button"
                  @click="deleteStudent(student.id)"
                  class="btn btn-danger"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
          <tbody v-else>
            <tr>
              <td colspan="7">Loading</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
