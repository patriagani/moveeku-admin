<template>
    <div class="dashboard">
        <p>dashboard</p>
        <Footer/>
    </div>
</template>

<script>
// import axios from 'axios'
import Footer from '@/components/Footer.vue'

export default {
  name: 'Dashboard',
  components: {
    Footer
  },
  
  data: () => {
    return {
      
    }
  },

  props: ['url'],
}
</script>

<style>

</style>