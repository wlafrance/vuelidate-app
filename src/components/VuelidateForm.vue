
<template>
  <v-container>
    <form class="ma-2">
      <v-text-field
        label="Name"
        v-model="name"
        :error-messages="nameErrors"
        :counter="10"
        @input="$v.name.$touch()"
        @blur="$v.name.$touch()"
        required
      ></v-text-field>
      <v-text-field
        label="E-mail"
        v-model="email"
        :error-messages="emailErrors"
        @input="$v.email.$touch()"
        @blur="$v.email.$touch()"
        required
      ></v-text-field>
      <v-select
        label="Item"
        v-model="select"
        :items="items"
        :error-messages="selectErrors"
        @change="$v.select.$touch()"
        @blur="$v.select.$touch()"
        required
      ></v-select>
      <v-checkbox
        label="Do you agree?"
        v-model="checkbox"
        :error-messages="checkboxErrors"
        @change="$v.checkbox.$touch()"
        @blur="$v.checkbox.$touch()"
        required
      ></v-checkbox>

      <v-btn @click="submit" color="success" class="mr-4">Submit</v-btn>
      <v-btn @click="clear" color="error" class="mr-4">Reset</v-btn>
    </form>
  </v-container>
</template>

<script>
import { required, maxLength, email } from "vuelidate/lib/validators";

export default {
  name: "HelloWorld",
  // BEGIN OF CODE
  validations: {
    name: { required, maxLength: maxLength(10) },
    email: { required, email },
    select: { required },
    checkbox: { required }
  },
  // BEGIN OF DATA
  data: () => ({
    name: "",
    email: "",
    select: null,
    items: ["Item 1", "Item 2", "Item 3", "Item 4"],
    checkbox: false
  }),
  // END OF DATA
  // BEGIN OF COMPUTED
  computed: {
    checkboxErrors() {
      const errors = [];
      if (!this.$v.checkbox.$dirty) return errors;
      !this.$v.checkbox.required && errors.push("You must agree to continue!");
      return errors;
    },
    selectErrors() {
      const errors = [];
      if (!this.$v.select.$dirty) return errors;
      !this.$v.select.required && errors.push("Item is required");
      return errors;
    },
    nameErrors() {
      const errors = [];
      if (!this.$v.name.$dirty) return errors;
      // !this.$v.name.decimal && errors.push('Name must be a number')
      !this.$v.name.maxLength &&
        errors.push("Name must be at most 10 characters long");
      !this.$v.name.required && errors.push("Name is required.");
      return errors;
    },
    emailErrors() {
      const errors = [];
      if (!this.$v.email.$dirty) return errors;
      !this.$v.email.email && errors.push("Must be valid e-mail");
      !this.$v.email.required && errors.push("E-mail is required");
      return errors;
    }
  },
  // END OF COMPUTED
  // BEGIN OF METHODS
  methods: {
    submit() {
      this.$v.$touch();
    },
    clear() {
      this.$v.$reset();
      this.name = "";
      this.email = "";
      this.select = null;
      this.checkbox = false;
    }
  }
  // END OF METHODS
  // END OF CODE
};
</script>
