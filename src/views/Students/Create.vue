<!-- @format -->

<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">Add Students</div>
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
          <button class="btn btn-primary" @click="saveStudent()" type="button">
            Save
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "studentCreate",
  data() {
    return {
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
  methods: {
    saveStudent() {
      var $this = this;
      axios
        .post("http://localhost:8000/api/students", this.model.student)
        .then((response) => {
          console.log(response.data);
          this.model.student = {
            name: "",
            course: "",
            email: "",
            phone: "",
          };

          this.errorList = "";
        })
        .catch(function (error) {
          if (error.response) {
            if (error.response.status == 422) {
              $this.errorList = error.response.data.errors;
            }
            // console.log(error.response.data);
            // console.log(error.response.status);
            // console.log(error.response.headers);
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
