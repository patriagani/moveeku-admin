<template>
    <div class="dashboard">
        <br><br>
        <h4>Admin Dashboard - Movie List</h4>
        <br>
        <div class="container">
          <div class="row">
            <div class="three columns">
              <strong><p>Title</p></strong>
              <p v-for="movie in movies" :key = "movie" style="margin-bottom: 10px; padding: 10px; font-size: 12px;">{{movie.title}}</p>
            </div>
            <div class="three columns">
              <strong><p>Price</p></strong>
              <p v-for="movie in movies" :key = "movie" style="margin-bottom: 10px; padding: 10px; font-size: 12px;">{{movie.price.toLocaleString()}} MVP</p>
            </div>
            <div class="three columns">
              <strong><p>Rating</p></strong>
              <p v-for="movie in movies" :key = "movie" style="margin-bottom: 10px; padding: 10px; font-size: 12px;">Imdb Rating: {{movie.data.imdbRating}}</p>
            </div>
            <div class="three columns">
              <strong><p>Action</p></strong>
              <div v-for="movie in movies" :key = "movie" style="display: flex; justify-content: center;">
                <button style="margin-left: 10px;" class="button-primary" @click="editMovie(movie._id)">Edit Movie</button>
                <button style="margin-left: 10px;" class="button" @click="deleteMovie(movie._id)">Delete Movie</button>
              </div>
            </div>
          </div>
        </div>
        <br><br>
        <Footer/>
    </div>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import Footer from '@/components/Footer.vue'

export default {
  name: 'Dashboard',
  components: {
    Footer
  },
  
  data: () => {
    return {
      movies: []
    }
  },

  props: ['url'],

  methods: {
    getMovies() {
      axios.get(`${this.url}/movies`)
        .then((response) => {
          this.movies = response.data
        })
        .catch((error) => {
          console.log(error.message)
        })
    },

    editMovie(movieId) {
      this.$router.push(`editmovie/${movieId}`)
    },

    deleteMovie(movieId) {

      const options = {
          method: 'DELETE',
          headers: {'x-auth-token': localStorage.getItem('token')},
          baseURL: `${this.url}/movies/${movieId}`
        }

      Swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!'
      }).then((result) => {
        if (result.value) {
          axios(options)
            .then(() => {
              Swal.fire(
                'Deleted!',
                'Your file movie been deleted.',
                'success'
              )
              this.getMovies()
            })
        }
        else {
          new Error('Cancel Delete Movie')
        }
      })
      .catch((error) => {
        console.log(error.message)
      })
    }
  },

  created() {
    this.getMovies()
  }
}
</script>

<style>

</style>