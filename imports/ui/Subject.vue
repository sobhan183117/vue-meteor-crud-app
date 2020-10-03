<template>
  <div class="student-add-section">
    <h2 class="text-center">Add Subject</h2>

    <form class="container" @submit.prevent="handleSubmit">
      <label for="name">Subject Name <span class="text-red"> *</span></label>
      <input
        type="text"
        v-model="subjectName"
        name="subjectName"
        placeholder="Enter subject name"
        required
      />

      <label for="name">Student <span class="text-red"> *</span></label>
      <input
        type="text"
        v-model="studentName"
        name="studentName"
        placeholder="Enter student"
        required
      />
      <input type="submit" name="submit" value="Add" />
    </form>
  </div>
</template>

<script>
// Client View imports
import Vue from "vue";

// Api collections imports
// import { Students } from "../api/collections/Student.js";
import { Subjects } from "../api/collections/Subject.js";

export default {
  data() {
    return {
      subjectName: "",
      studentName: "",
      errors: {},
    };
  },
  methods: {
    handleSubmit() {
      console.log("validForm", this.validForm());

      if (this.validForm()) {
        Subjects.insert({
          subjectName: this.subjectName,
          studentName: this.studentName,
          createdAt: new Date(), // current time
        });

        // Clear form
        this.subjectName = "";
        this.studentName = "";
      }
    },

    validForm() {
      let isFormValid = true;
      this.errors = {};

      if (!this.subjectName) {
        this.errors["subjectName"] = " (field is required.)";
      }

      if (!this.studentName) {
        this.errors["studentName"] = " (field is required.)";
      }

      if (Object.keys(this.errors).length > 0) {
        isFormValid = false;
      }

      // console.log("errors", this.errors, isFormValid);

      return isFormValid;
    },
  },
};
</script>

<style></style>
