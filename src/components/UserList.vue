<template>
  <div id="userlist">
    <h1>Component UserList</h1>
    <v-card class="elevation-12 pa-3">
      <v-layout row wrap class="pa-3" v-for="user in sortedUsers" :key="user.id">

        <template v-if="userId === user.id">
          <v-flex xs12 md3>
            <div class="caption grey--text">First Name</div>
            <div>
              <v-text-field
              v-model="editUserData.firstname"
              required
              >
              </v-text-field>
            </div>
          </v-flex>
          <v-flex xs6 sm4 md3>
            <div class="caption grey--text">Last Name</div>
            <div>
              <v-text-field
              v-model="editUserData.lastname"
              required
              >
              </v-text-field>
            </div>
          </v-flex>
          <v-flex xs6 sm4 md3>
            <div class="caption grey--text">Job Title</div>
            <div>
              <v-text-field
              v-model="editUserData.jobtitle"
              required
              >
              </v-text-field>
            </div>
          </v-flex>
          <v-flex xs6 sm4 md2>
            <div>
              <v-btn
                flat
                icon
                color="pink"
                @click="onEditSubmit(user)"
              >
                <v-icon>check</v-icon>
              </v-btn>
              <v-btn
                flat
                icon
                color="pink"
                @click="onEditCancel()"
              >
                <v-icon>cancel</v-icon>
              </v-btn>
            </div>
          </v-flex>
        </template>

        <template v-else>
          <v-flex xs12 md3>
            <div class="caption grey--text">First Name</div>
            <div>{{ user.firstname }}</div>
          </v-flex>
          <v-flex xs6 sm4 md3>
            <div class="caption grey--text">Last Name</div>
            <div>{{ user.lastname }}</div>
          </v-flex>
          <v-flex xs6 sm4 md3>
            <div class="caption grey--text">Job Title</div>
            <div>{{ user.jobtitle }}</div>
          </v-flex>
          <v-flex xs6 sm4 md2>
            <div>
              <v-btn
                flat
                icon
                color="pink"
                @click="onEdit(user)"
              >
                <v-icon>edit</v-icon>
              </v-btn>
              <v-btn
                flat
                icon
                color="pink"
                @click="onDelete(user)"
              >
                <v-icon>delete_forever</v-icon>
              </v-btn>
            </div>
          </v-flex>
        </template>

      </v-layout>
    </v-card>
  </div>
</template>

<script>
import db from '@/fb'

export default {
  data() {
    return {
      users: [],
      userId: '',
      editUser: '',
      editUserData: {
        firstname: '',
        lastname: '',
        jobtitle: ''
      }
    }
  },
  methods: {
    onDelete: function(user) {
      db.collection("users")
        .doc(user.id).delete()
    },
    onEdit (user) {
      this.userId = user.id
      this.editUserData.firstname = user.firstname,
      this.editUserData.lastname = user.lastname,
      this.editUserData.jobtitle = user.jobtitle
    },
    onEditSubmit (user) {
      db.collection('users')
        .doc(user.id).update({
          firstname: this.editUserData.firstname,
          lastname: this.editUserData.lastname,
          jobtitle: this.editUserData.jobtitle
        })
        .then(
          this.onEditCancel()
        )
    },
    onEditCancel() {
      this.userId = ''
      this.editUserData.firstname = ''
      this.editUserData.lastname = ''
      this.editUserData.jobtitle = ''
    }
  },
  computed:{
    // sortedUsers () {
      // return this.users.slice().sort(function(a, b) {
      //  var nameA=a.firstname.toLowerCase(), nameB=b.firstname.toLowerCase();
      //   if (nameA < nameB)
      //     return -1;
      //   if (nameA > nameB)
      //     return 1;
      //   return 0;
    //   });
    // }
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

        if (change.type === 'modified'){
          const editedUser = this.users.find(
            user => user.id === change.doc.id
          );

          const objEdited = change.doc.data()
          objEdited.id = change.doc.id

          const index = this.users.indexOf(editedUser);
          this.users.splice(index, 1, objEdited)
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
