<template>
    <div class="dashboard">
        <br><br>
        <h4>Edit Movie</h4>
        <h4>{{title}}</h4>
        <h4></h4>
        <div class="container" style="padding-left:200px; padding-right: 200px; padding-bottom: 50px;">
            <form align="left">
                <label for="exampleEmailInput">Price</label>
                <input v-model="price" class="u-full-width" type="number" placeholder="0" id="exampleNumberInput">
                <label for="exampleEmailInput">WatchLink</label>
                <input v-model="watchlink" class="u-full-width" type="text" placeholder="Watchlink URL" id="exampleTextInput">
                <input @click.prevent="editMovie" class="button-primary" type="submit" value="Save Movie">
            </form>
        </div>
        <Footer/>
    </div>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import Footer from '@/components/Footer.vue'

export default {
  name: 'EditMovie',
  components: {
    Footer
  },
  
  data: () => {
    return {
      title: "",
      price: 1000,
      watchlink: ""
    }
  },

  props: ['url'],

  methods: {
    editMovie() {
      let obj = {
        price: this.price,
        watchlink: this.watchlink
      }

      const options = {
          method: 'PATCH',
          headers: {'x-auth-token': localStorage.getItem('token')},
          baseURL: `${this.url}/movies/${this.$route.params.movieId}`,
          data: obj
        }

      axios(options)
        .then(() => {
          Swal.fire({
            icon: 'success',
            title: 'Yeaay',
            text: 'Movie Saved',
          })
          this.$router.push('/dashboard')
        })
        .catch((error) => {
          console.log(error.message)
        })
    },
    getMovie() {
      axios.get(`${this.url}/movies/${this.$route.params.movieId}`)
        .then((response) => {
          this.title = response.data.title
          this.price = response.data.price
          this.watchlink = response.data.watchlink
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
            this.getMovie()
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

<style>

</style>