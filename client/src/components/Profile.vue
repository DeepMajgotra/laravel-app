<template>
  <div class="container">
    <div class="jumbotron mt-5">
      <div class="col-sm-8 mx-auto">
        <h1 class="text-center">Profile</h1>
      </div>

      <table class="table col-md-6 mx-auto">
        <tbody>
          <tr>
            <td>Name</td>
            <td>{{ wholename }}</td>
          </tr>
          <tr>
            <td>Email</td>
            <td>{{ email }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: () => ({
    wholename: "",
    email: ""
  }),

  mounted() {
    this.getUser();
  },

  methods: {
    
    getUser() {
      axios
        .get("api/profile", {
          headers: {
            Authorization: `Bearer ${localStorage.usertoken}`
          }
        })
        .then(res => {
          this.wholename = res.data.user.name;
          this.email = res.data.user.email;
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>
