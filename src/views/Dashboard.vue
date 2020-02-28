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
                <button style="margin-left: 10px;" class="button" @click="deleteMovie">Delete Movie</button>
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
    }
  },

  created() {
    this.getMovies()
  }
}
</script>

<style>

</style>