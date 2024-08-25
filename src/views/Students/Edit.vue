<!-- @format -->

<script>
import axios from "axios";

export default {
  name: "studentEdit",
  data() {
    return {
      studentId: "",
      errorList: "",
      model: {
        student: {
          name: "",
          course: "",
          email: "",
          phone: "",
        },
      },
    };
  },
  mounted() {
    // console.log(this.$route.params.id);
    this.studentId = this.$route.params.id;
    this.getStudentData(this.$route.params.id);
  },
  methods: {
    getStudentData(studentId) {
      axios
        .get(`http://localhost:8000/api/students/${studentId}/edit`)
        .then((response) => {
          this.model.student = response.data.student;
        })
        .catch(function (error) {
          if (error.response) {
            if (error.response.status == 404) {
              console.log(error.response.data.message);
            }
          }
        });
    },
    updateStudent() {
      var $this = this;
      axios
        .put(
          `http://localhost:8000/api/students/${this.studentId}/edit`,
          this.model.student
        )
        .then((response) => {
          console.log(response.data);
          this.errorList = "";
        })
        .catch(function (error) {
          if (error.response) {
            if (error.response.status == 422) {
              $this.errorList = error.response.data.errors;
            }

            if (error.response.status == 404) {
              console.log(error.response.data.message);
            }
          } else if (error.request) {
            console.log(error.request);
          } else {
            console.log("Error", error.message);
          }
          console.log(error.config);
        });
    },
  },
};
</script>

<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">Edit Students</div>
      <div class="card-body">
        <ul v-if="Object.keys(this.errorList).length > 0">
          <li
            class="alert alert-warning"
            v-for="(error, index) in this.errorList"
            :key="index"
          >
            {{ error[0] }}
          </li>
        </ul>
        <!-- Name -->
        <div class="mb-3">
          <label for="name" class="form-label">Name</label>
          <input
            type="text"
            class="form-control"
            v-model="model.student.name"
          />
        </div>

        <!-- Course -->
        <div class="mb-3">
          <label for="name" class="form-label">Course</label>
          <input
            type="text"
            class="form-control"
            v-model="model.student.course"
          />
        </div>

        <!-- Email -->
        <div class="mb-3">
          <label for="name" class="form-label">Email</label>
          <input
            type="text"
            class="form-control"
            v-model="model.student.email"
          />
        </div>

        <!-- Phone -->
        <div class="mb-3">
          <label for="name" class="form-label">Phone</label>
          <input
            type="text"
            class="form-control"
            v-model="model.student.phone"
          />
        </div>

        <div class="mb-3">
          <button class="btn btn-primary" @click="updateStudent" type="button">
            Update
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
