<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark rounded">
    <div class="navbar-collapse justify-content-md-center" id="navbar1">
      <ul class="navbar-nav">
        <li class="nav-item">
          <router-link class="nav-link" to="/">Home</router-link>
        </li>
        <li v-if="auth==''" class="nav-item">
          <router-link class="nav-link" to="/login">Login</router-link>
        </li>
        <li v-if="auth==''" class="nav-item">
          <router-link class="nav-link" to="/register">Register</router-link>
        </li>
        <li v-if="auth=='loggedin'" class="nav-item">
          <router-link class="nav-link" to="/profile">Profile</router-link>
        </li>
        <li v-if="auth=='loggedin'" class="nav-item">
          <a class="nav-link" @click="logout" href="#">Logout</a>
        </li>
      </ul>
    </div>
  </nav>
</template>


<script>
    import EventBus from './EventBus';
    export default {
        data() {
            return {
                auth: '',
                user: '',
            }
        },

        methods:{
              logout() {
                  localStorage.removeItem('usertoken')
                  this.$router.push('/');
                  this.auth = '';
              }
            },

            mounted() {
                if (localStorage.getItem('usertoken')) {
                  this.auth = 'loggedin';
                } else {
                  this.auth = '';
                }
                EventBus.$on('logged-in', status => {
                    console.log(status)
                    this.auth = status
                })
            }
        }

</script>