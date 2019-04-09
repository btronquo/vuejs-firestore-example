<template>
  <v-app>
    <Users/>
    <Footer/>
  </v-app>
</template>

<script>
import Users from './views/Users'

import Footer from './components/Footer'

export default {
  name: 'App',
  components: {
    Users,
    Footer
  },
  data () {
    return {
      //
    }
  }
}
</script>
