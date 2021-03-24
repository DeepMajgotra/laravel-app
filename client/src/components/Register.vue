<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6 mt-5 mx-auto">
        <form v-on:submit.prevent="register">
          <h1 class="h3 mb-3 font-weight-normal">Register</h1>

          <div class="form-group">
            <label for="first_name"> First Name</label>
            <input
              type="text"
              v-model="first_name"
              class="form-control"
              name="first_name"
              placeholder="first name"
            />
            <span class="text-danger" v-if='errors.first_name'>
              {{ errors.first_name }}
            </span>
          </div>

          <div class="form-group">
            <label for="last_name"> Last Name</label>
            <input
              type="text"
              v-model="last_name"
              class="form-control"
              name="last_name"
              placeholder="last name"
            />
            <span class="text-danger" v-if='errors.last_name'>
              {{ errors.last_name }}
            </span>
          </div>

          <div class="form-group">
            <label for="email"> Email Address</label>
            <input
              type="email"
              v-model="email"
              class="form-control"
              name="email"
              placeholder="Email Address"
            />
            <span class="text-danger" v-if='errors.email'>
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
            <span class="text-danger" v-if='errors.password'>
              {{ errors.password }}
            </span>
          </div>

          <button class="btn btn-lg btn-primary btn-block">Register</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import router from "../router";

export default {
  data() {
    return {
      first_name: "",
      last_name: "",
      email: "",
      password: "",
      errors: {
        first_name: "",
        last_name: "",
        email: "",
        password: ""
      },
      liveCheck: false,
    };
  },

  methods: {
    clearMessages() {
      const { errors } = this;
      errors.first_name = "";
      errors.last_name = "";
      errors.email = "";
      errors.password = "";

      this.errors = errors;
    },
    validateEmail(email) {
      const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(String(email).toLowerCase());
    },
    validate() {
      const { first_name, last_name, email, password } = this;
      const { errors } = this;
      this.clearMessages();
      this.liveCheck = true;
      if (!first_name) errors.first_name = "First name is required";
      if (!last_name) errors.last_name = "First name is required";
      if (!email) errors.email = "First name is required";
      if (!password) errors.password = "First name is required";

      if (password.split("").length < 8)
        errors.password = "Password needs atleast 8 characters";
      if (!this.validateEmail(email))
        errors.email = "Please enter correct email";
      
      this.errors = errors;
      return !errors.first_name && !errors.last_name && !errors.email && !errors.password;
    },
    register() {
      if (!this.validate()) return false;
      axios
        .post("/api/register", {
          name: this.first_name + " " + this.last_name,
          email: this.email,
          password: this.password
        })
        .then(res => {
          if (res.data.email) {
            this.errors.password = 'Email already in use';
          } else {
          router.push({ name: "Login" });
          }
        })
        .catch(err => {
          this.errors.password = 'Failed to register';
          console.log({err});
        });
    }
  },
  watch: {
    first_name() {
      if (this.liveCheck) {
        this.validate();
      }
    },
    last_name() {
      if (this.liveCheck) {
        this.validate();
      }
    },
    email() {
      if (this.liveCheck) {
        this.validate();
      }
    },
    password() {
      if (this.liveCheck) {
        this.validate();
      }
    },
  }
};
</script>
