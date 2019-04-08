<template>
  <div id="userform">
    <h1>Component UserAdd</h1>
    <v-layout row wrap>
    <v-card class="elevation-12">
      <v-toolbar dark color="primary">
        <v-toolbar-title>Add a person</v-toolbar-title>
        <v-spacer></v-spacer>
      </v-toolbar>
      <v-card-text>
        <v-form
          ref="form"
          v-model="valid"
          lazy-validation
        >
          <v-text-field
            v-model="firstname"
            :rules="nameRules"
            label="First name"
            required
          ></v-text-field>
          <v-text-field
            v-model="lastname"
            :rules="nameRules"
            label="Last name"
            required
          ></v-text-field>
          <v-text-field
            v-model="jobtitle"
            :rules="jobtitleRules"
            label="Job title"
            required
          ></v-text-field>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          :disabled="!valid"
          color="primary"
          @click="submit"
          :loading="addLoading"
        >+ Add</v-btn>
        </v-card-actions>
      </v-card>
    </v-layout>
  </div>
</template>

<script>
import db from '@/fb'

export default {
  data: () => ({
    valid: true,
    firstname: '',
    lastname: '',
    jobtitle: '',
    addLoading: false,
    nameRules: [
      v => !!v || 'This field is required'
    ],
    jobtitleRules: [
      v => !!v || 'Please enter a job title'
    ]
  }),
  methods: {
    submit: function() {
      if (this.$refs.form.validate()) {
        this.addLoading = true;
        const user = {
          firstname: this.firstname,
          lastname: this.lastname,
          jobtitle: this.jobtitle
        }
        db.collection('users').add(user).then(() => {
          this.addLoading = false;
          this.$refs.form.reset()
        })
      }
    },
  },
}
</script>
