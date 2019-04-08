<template>
  <div id="users">
    <div class="pa-3">
      <h1 >vuejs-firestore-example</h1>
      <p>Hi, I made this quick project to better understand how firestore and vuejs can work together.<br>
        Free feel to contact me on <b>github</b> if you have any question! <br>
        <b>Boris Tronquoy</b>
      </p>
    </div>
    <v-container class="my-5">
      <v-layout row justify-center>
      <v-flex xs4>
      <UserForm/>
      </v-flex>
      <v-flex xs8>
      <UserList/>
      </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import UserForm from '@/components/UserForm'
import UserList from '@/components/UserList'

export default {
  components: {
    UserForm,
    UserList
  },
  data () {
    return {
      //
    }
  }
}
</script>
