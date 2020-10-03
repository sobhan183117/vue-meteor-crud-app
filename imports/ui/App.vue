<template>
  <div class="app-wrapper">
    <Student />
    <ul>
      <StudentList
        v-for="(student, id) in getStudents"
        v-bind:key="id"
        v-bind:student="student"
      />
    </ul>

    <Subject />
    <ul>
      <SubjectList
        v-for="(subject, id) in getSubjects"
        v-bind:key="id"
        v-bind:subject="subject"
      />
    </ul>
  </div>
</template>

<script>
// Client view imports
import Vue from "vue";
import Student from "./Student.vue";
import StudentList from "./StudentList.vue";
import Subject from "./Subject.vue";
import SubjectList from "./SubjectList.vue";

// Api collections imports
import { Students } from "../api/collections/Student.js";
import { Subjects } from "../api/collections/Subject.js";

export default {
  components: {
    Student,
    StudentList,
    Subject,
  },
  data() {
    return {};
  },
  methods: {},
  meteor: {
    getStudents() {
      return Students.find({}, { sort: { createdAt: -1 } }).fetch();
    },
    getSubjects() {
      return Subjects.find({}, { sort: { createdAt: -1 } }).fetch();
    },
  },
};
</script>

<style>
.student-add-section {
  width: 100%;
  padding-top: 10px;
}

.container {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 15px 35px;
  margin-bottom: 25px;
}
</style>
