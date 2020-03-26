<template>
    <div class="dashboard">
        <br><br>
        <h4>Add Movie</h4>
        <div class="container" style="padding-left:200px; padding-right: 200px; padding-bottom: 50px;">
            <form align="left">
                <label for="exampleEmailInput">Imdb Id</label>
                <input v-model="imdbid" class="u-full-width" type="text" placeholder="Movie Imdb Id" id="exampleTextInput">
                <label for="exampleEmailInput">Price</label>
                <input v-model="price" class="u-full-width" type="number" placeholder="0" id="exampleNumberInput">
                <label for="exampleEmailInput">WatchLink</label>
                <input v-model="watchlink" class="u-full-width" type="text" placeholder="Watchlink URL" id="exampleTextInput">
                <input @click.prevent="createMovie" class="button-primary" type="submit" value="Post Movie">
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
  name: 'AddMovie',
  components: {
    Footer
  },
  
  data: () => {
    return {
      imdbid: "",
      price: 1000,
      watchlink: ""
    }
  },

  props: ['url'],

  methods: {
    createMovie() {
      let obj = {
        imdbid: this.imdbid,
        price: this.price,
        watchlink: this.watchlink
      }

      const options = {
          method: 'POST',
          headers: {'x-auth-token': localStorage.getItem('token')},
          baseURL: `${this.url}/movies`,
          data: obj
        }

      axios(options)
        .then(() => {
          Swal.fire({
            icon: 'success',
            title: 'Yeaay',
            text: 'Movie Added',
          })
          this.imdbid = "",
          this.price = 1000,
          this.watchlink = ""
        })
        .catch((error) => {
          Swal.fire({
            icon: 'error',
            title: 'Oops...',
            text: 'Something went wrong! '+ error.message,
          })
        })
    },

    adminCheck() {
      const options = {
          method: 'GET',
          headers: {'x-auth-token': localStorage.getItem('token')},
          baseURL: `${this.url}/users/admin/admincheck`
      }

      axios(options)
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