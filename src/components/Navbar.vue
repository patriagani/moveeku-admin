<template>
  <div class="navbar">
    <nav class="nav-show">
        <div class="container">
            <ul>
                <li><router-link :to="{ path: '/' }">Home</router-link></li>
                <li v-if="isLogin" ><router-link :to="{ path: '/dashboard' }">Dashboard</router-link></li>
                <li v-if="isLogin" ><router-link :to="{ path: '/addmovie' }">Add Movie</router-link></li>
                
                <li v-if="!isLogin" class="u-pull-right"><router-link :to="{ path: '/login' }">Login</router-link></li>
                <li v-if="!isLogin" class="right"><router-link :to="{ path: '/signup' }">Register</router-link></li>
                <li v-if="isLogin" class="right"><router-link :to="{ path: '/' }" v-on:click.native="signOut()" >Sign Out</router-link></li>
                <li class="icon">
                    <a href="javascript:void(0);" onclick="toggleMobileNav();">&#9776;</a>
                </li>
            </ul>
        </div>
    </nav>
  </div>
</template>

<script>
import axios from 'axios'

export default {

  data: () => {
      return {
          
      }
  },

  props: ['url', 'isLogin'],

  methods: {

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
    },

    signOut() {
        localStorage.clear()
        this.$emit('adminlogout')
    }
  },

  created() {
      this.adminCheck()
  }


}
</script>>
