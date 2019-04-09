<template>
  <div id="userlist">
    <h1>Component UserList</h1>
    <v-card class="elevation-12">
      <v-layout row wrap class="pa-3" v-for="user in sortedUsers" :key="user.id">
        <v-flex xs2>
          <div class="caption grey--text">Firstname</div>
          <div>{{ user.firstname }}</div>
        </v-flex>
        <v-flex xs2>
          <div class="caption grey--text">Lastname</div>
          <div>{{ user.lastname }}</div>
        </v-flex>
        <v-flex xs3>
          <div class="caption grey--text">Job title</div>
          <div>{{ user.jobtitle }}</div>
        </v-flex>
        <v-flex xs2>
          <v-tooltip bottom>
            <template v-slot:activator="{ on }">
              <v-btn flat icon color="red">
                <v-icon
                color="primary"
                dark v-on="on"
                @click="onEdit(user)"
                >
                  edit
                </v-icon>
              </v-btn>
            </template>
            <span>Edit</span>
          </v-tooltip>
          <v-tooltip bottom>
            <template v-slot:activator="{ on }">
              <v-btn flat icon color="red">
                <v-icon
                color="primary"
                dark v-on="on"
                @click="onDelete(user)"
                >
                  delete_forever
                </v-icon>
              </v-btn>
            </template>
            <span>Delete</span>
          </v-tooltip>
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
      users: []
    }
  },
  methods: {
    onDelete: function(user) {
      db.collection("users")
        .doc(user.id).delete()
    }
  },
  computed:{
    sortedUsers () {
      return this.users.slice().sort(function(a, b){
      //  var nameA=a.firstname.toLowerCase(), nameB=b.firstname.toLowerCase();
      //   if (nameA < nameB)
      //     return -1;
      //   if (nameA > nameB)
      //     return 1;
      //   return 0;
      });
    }
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

        if (change.type === "removed") {
          const deletedUser = this.users.find(
            user => user.id === change.doc.id
          );
          const index = this.users.indexOf(deletedUser);
          this.users.splice(index, 1);
          this.index = this.index === 0 ? 0 : index - 1;
        }
      })
    })
  }
}
</script>
