<template>
    <div class="Login">
        <br><br>
        <h4>Login to Your Account</h4>
        <p align="center">Doesn't have an account? Sign up <router-link :to="{ path: '/signup' }">here</router-link></p>
        <div class="container" style="padding-left:200px; padding-right: 200px; padding-bottom: 50px;">
            <form align="left">
                <label for="exampleEmailInput">Your email</label>
                <input v-model="email" class="u-full-width" type="email" placeholder="Your E-mail" id="exampleEmailInput">
                <label for="exampleEmailInput">Your Password</label>
                <input v-model="password" class="u-full-width" type="password" placeholder="Your Password" id="examplePasswordInput">
                <input @click.prevent="loginAdmin" class="button-primary" type="submit" value="Login">
            </form>
        </div>
        <Footer/>
    </div>
</template>

<script>
import axios from 'axios'
import Footer from '@/components/Footer.vue'

export default {
  name: 'Login',
  components: {
    Footer
  },
  
  data: () => {
    return {
      email: "",
      password: ""
    }
  },

  props: ['url'],

  methods: {
    loginAdmin () {
      let obj = {
        email: this.email,
        password: this.password
      }
      axios.post(`${this.url}/users/admin/signin`, obj)
        .then((response) => {
          localStorage.setItem('token', response.headers['x-auth-token'])
          localStorage.setItem('id', response.data.id)
          localStorage.setItem('username', response.data.username)
          this.$router.push('/dashboard')
          this.$emit('adminlogin')
        })
        .catch((error) => {
          console.log(error.message)
        })
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
            this.$router.push('dashboard')
          }
        })
    }
  },

  created() {
    this.adminCheck()
  }

}
</script>

<style>

</style>