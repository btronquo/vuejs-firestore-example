<template>
  <v-app>
    <Users/>
  </v-app>
</template>

<script>
import Users from './views/Users'

export default {
  name: 'App',
  components: {
    Users
  },
  data () {
    return {
      //
    }
  }
}
</script>
