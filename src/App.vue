<template>
  <div id="app">
    <Navbar @adminlogout="adminLogout" :isLogin="isLogin" :url="url"/>
    <router-view  @adminlogin="adminLogin" :isLogin="isLogin" :url="url"/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>

<script>
// @ is an alias to /src
import Navbar from '@/components/Navbar.vue'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    Navbar
  },

  data: () => {
    return {
      url: 'http://localhost:3000',
      isLogin: false
    }
  },

  methods: {
    adminLogin() {
      this.isLogin = true
    },

    adminLogout() {
      this.isLogin = false
    },

    adminCheck() {
      const options = {
          method: 'GET',
          headers: {'x-auth-token': localStorage.getItem('token')},
          baseURL: `${this.url}/users/admin/admincheck`
      }

      axios(options)
        .then((response) => {
          if (response.data.admin) {
            this.isLogin = true
          }
        })
        .catch(() => {
          this.$router.push('login')
        })
    }
  },

  created() {
    this.adminCheck()
  }
}
</script>

