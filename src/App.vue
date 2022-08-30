<template>
<div>
  <Nav/>
  <router-view/>

  <Footer/>
</div>
</template>

<script>
import Nav from '@/components/Nav'
import Footer from '@/components/Footer'
import axios from 'axios'
export default {
  name: 'App',
  components: {
    Nav,
    Footer
  },
  // lifecycle hook to import mutations in vue
  // store refers to vuex store
  beforeCreate() {
    this.$store.commit('initializeStore')

    // checks if token is set
    const token = this.$store.state.user.token

    if (token) {
      // creates a string with axios and adds the token to it
      axios.defaults.headers.common['Authorization'] = "Token " + token
    } else {
      axios.defaults.headers.common['Authorization'] = ""
    }
  }
}
</script>

<style lang="scss">
@import '../node_modules/bulma';
</style>
