<template>
    <form v-on:submit.prevent="subStart" class="">

        <input type="text" v-validate="'required|email'" data-vv-validate-on="change" name="email" placeholder="Your Email" v-model="email" class="columns" required>

        <input type="text" placeholder="Your First Name" class="columns" v-model="firstName" required >
        <input type="text" placeholder="Your Last Name" class="columns" v-model="lastName" required>
        <span>{{ errors.first('email') }}</span>

        <input type="submit" class="button" :disabled="errors.any()">
    </form>
</template>

<script>
export default {
  name: "Welcome",
  data: function() {
    return {
      firstName: "",
      lastName: "",
      email: ""
    };
  },
  methods: {
    subStart() {
      this.$emit("startClick", {
        firstName: this.firstName,
        lastName: this.lastName,
        email: this.email
      });
    }
  },
  props: ["qCount"]
};
</script>

<style>
</style>

