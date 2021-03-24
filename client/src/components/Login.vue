<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6 mt-5 mx-auto">
        <form v-on:submit.prevent="login">
          <h1 class="h3 mb-3 font-weight-normal">Please sign in</h1>

          <div class="form-group">
            <label for="email"> Email Address</label>
            <input
              type="email"
              v-model="email"
              class="form-control"
              name="email"
              placeholder="Email Address"
            />
            <span class="text-danger" v-if="errors.email">
              {{ errors.email }}
            </span>
          </div>

          <div class="form-group">
            <label for="password"> Password</label>
            <input
              type="password"
              v-model="password"
              class="form-control"
              name="password"
              placeholder="Password"
            />
            <span class="text-danger" v-if="errors.password">
              {{ errors.password }}
            </span>
          </div>

          <button class="btn btn-lg btn-primary btn-block">Sign in</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import router from "../router";
import EventBus from "./EventBus";

export default {
  data() {
    return {
      email: "",
      password: "",
      errors: {
        email: "",
        password: ""
      }
    };
  },

  methods: {
    validate() {
      const { email, password } = this;
      const { errors } = this;
      this.clearMessages();
      if (!email) errors.email = "First name is required";
      if (!password) errors.password = "First name is required";

      return !errors.email && !errors.password;
    },
    clearMessages() {
      const { errors } = this;
      errors.first_name = "";
      errors.last_name = "";
      errors.email = "";
      errors.password = "";

      this.errors = errors;
    },
    login() {
      if (!this.validate()) return false;
      axios
        .post("/api/login", {
          email: this.email,
          password: this.password
        })
        .then(res => {
          if (res.data.token) {
            this.emitMethod();
            localStorage.setItem("usertoken", res.data.token);
          }
          this.email = "";
          this.password = "";
          router.push({ name: "Profile" });
        })
        .catch(err => {
          this.errors.password = 'Invalid Credentials';
        });
    },

    emitMethod() {
      EventBus.$emit("logged-in", "loggedin");
    }
  }
};
</script>
