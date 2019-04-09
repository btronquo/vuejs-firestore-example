<template>
  <div id="footer">
    <v-container>
      <p>
      Made with <v-icon>favorite</v-icon> by Boris <strong>Tronquoy</strong> | #firestore #vuetify
      </p>
    </v-container>
   

  </div>
</template>

<script>
export default {
  data: () => ({
    copyright: 'hello'
  })
}
</script>

<style>
footer {
 position: absolute;
 bottom: 0;
 width: 100%;
 background: none
}
</style>