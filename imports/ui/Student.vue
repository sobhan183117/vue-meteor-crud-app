<template>
  <div>
    <div class="student-add-section">
      <h2 class="text-center">Add New Student</h2>

      <form class="container" @submit.prevent="handleSubmit">
        <label for="name"
          >Name <span class="text-red"> * {{ errors["name"] }}</span></label
        >
        <input
          type="text"
          name="name"
          placeholder="Enter name"
          required
          v-model="name"
        />

        <label for="email"
          >Email <span class="text-red"> * {{ errors["email"] }}</span></label
        >
        <input
          type="email"
          name="email"
          placeholder="Enter email"
          required
          v-model="email"
        />

        <label for="phone"
          >Phone No.
          <span class="text-red"> * {{ errors["phone"] }}</span></label
        >
        <input
          type="text"
          name="phone"
          placeholder="Enter phone no. (Ex. 017xxxxxxxx)"
          required
          v-model="phone"
          @input="acceptNumber"
        />

        <label for="dob"
          >Date of Birth
          <span class="text-red"> * {{ errors["dob"] }}</span></label
        >
        <input
          type="date"
          name="dob"
          placeholder="Date of birth"
          required
          v-model="dob"
        />

        <input type="submit" name="submit" value="Add" />
      </form>
    </div>
  </div>
</template>

<script>
// Client View imports
import Vue from "vue";

// Api collections imports
import { Students } from "../api/collections/Student.js";

export default {
  data() {
    return {
      name: "",
      email: "",
      phone: "",
      dob: "",
      errors: {},
    };
  },
  methods: {
    acceptNumber() {
      var x = this.phone
        .replace(/\D/g, "")
        .match(/(\d{0,3})(\d{0,4})(\d{0,4})/);
      this.phone = !x[2]
        ? x[1]
        : "(" + x[1] + ") " + x[2] + (x[3] ? "-" + x[3] : "");
    },

    validEmail: function (email) {
      var re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    },

    handleSubmit() {
      // console.log("validForm", this.validForm());

      if (this.validForm()) {
        Students.insert({
          name: this.name,
          email: this.email,
          phone: this.phone,
          dob: this.dob,
          createdAt: new Date(), // current time
        });

        // Clear form
        this.name = "";
        this.email = "";
        this.phone = "";
        this.dob = "";
      }
    },

    validForm() {
      let isFormValid = true;
      this.errors = {};

      if (!this.name) {
        this.errors["name"] = " (field is required.)";
      }

      if (!this.email) {
        this.errors["email"] = " (field is required.)";
      } else if (!this.validEmail(this.email)) {
        this.errors["email"] = " (Valid email is required.)";
      }

      if (!this.phone) {
        this.errors["phone"] = "(field is required.)";
      } else if (!this.validOperatorCode(this.phone)) {
        this.errors["phone"] = " (Valid phone no. is required.)";
      }

      if (!this.dob) {
        this.errors["dob"] = " field is required.";
      }

      if (Object.keys(this.errors).length > 0) {
        isFormValid = false;
      }

      // console.log("errors", this.errors, isFormValid);

      return isFormValid;
    },

    validOperatorCode: function (phone) {
      var opCode = phone.substring(1, 4);
      // console.log("opCode", opCode);

      if (
        ["011", "015", "016", "018", "017", "013", "019", "014"].includes(
          opCode
        ) &&
        phone.length > 14
      ) {
        console.log("num true");
        return true;
      }
    },
  },
};
</script>

<style></style>
