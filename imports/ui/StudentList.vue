<template>
  <li v-bind:class="studentClassName">
    <button className="delete" @click="deleteStudent">×</button>

    <input
      type="checkbox"
      readOnly
      v-bind:checked="!!this.student.checked"
      @click="toggleChecked"
    />
    <span class="text">{{ this.student }}</span>
  </li>
</template>

<script>
// Api collections imports
import { Students } from "../api/collections/Student.js";

export default {
  props: ["student"],
  data() {
    return {};
  },
  computed: {
    studentClassName: function () {
      return this.student.checked ? "checked" : "";
    },
  },

  methods: {
    toggleChecked() {
      // Set the checked property to the opposite of its current value
      Students.update(this.student._id, {
        $set: { checked: !this.student.checked },
      });
    },
    deleteStudent() {
      Students.remove(this.student._id);
    },
  },
};
</script>
