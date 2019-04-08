<template>
  <div id="userlist">
    <h1>Component UserList</h1>
    <v-card class="elevation-12">
      <v-layout row wrap class="pa-3" v-for="user in users" :key="user.id">
        <v-flex xs6 sm4 md2>
          <div class="caption grey--text">Firstname</div>
          <div>{{ user.firstname }}</div>
        </v-flex>
        <v-flex xs6 sm4 md2>
          <div class="caption grey--text">Lastname</div>
          <div>{{ user.lastname }}</div>
        </v-flex>
        <v-flex xs6 sm4 md2>
          <div class="caption grey--text">Job title</div>
          <div>{{ user.jobtitle }}</div>
        </v-flex>
      </v-layout>
      <v-divider></v-divider>
    </v-card>
  </div>
</template>

<script>
import db from '@/fb'
    export default {
      data() {
        return {
          users: [],
          items: [
            { message: 'Foo' },
            { message: 'Bar' }
          ]
        }
      },
      methods: {
      },
      created() {
        db.collection('users').onSnapshot(res =>{
          const changes = res.docChanges();

          changes.forEach(change => {
            if (change.type === 'added'){
              this.users.push({
                ...change.doc.data(),
                id: change.doc.id
              })
            }
          })
        })
      }
  }
</script>
