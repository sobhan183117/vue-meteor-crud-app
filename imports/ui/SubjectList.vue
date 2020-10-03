<template>
  <li v-bind:class="subjectClassName">
    <button className="delete" @click="deleteSubject">×</button>

    <input
      type="checkbox"
      readOnly
      v-bind:checked="!!this.subject.checked"
      @click="toggleChecked"
    />
    <span class="text">{{ this.subject }}</span>
  </li>
</template>

<script>
// Api collections imports
import { Subjects } from "../api/collections/Subject.js";

export default {
  props: ["subject"],
  data() {
    return {};
  },
  computed: {
    subjectClassName: function () {
      return this.subject.checked ? "checked" : "";
    },
  },

  methods: {
    toggleChecked() {
      // Set the checked property to the opposite of its current value
      Subjects.update(this.subject._id, {
        $set: { checked: !this.subject.checked },
      });
    },
    deleteSubject() {
      Subjects.remove(this.subject._id);
    },
  },
};
</script>
