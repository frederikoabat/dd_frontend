<template>
  <v-row class="pa-4" align="center">
    <v-col cols="2">
      <v-img
        :src="require('../assets/logo_big.svg')"
        height="40"
        @click="$emit('ZurueckZurStartseite')"
      >
      </v-img>
    </v-col>
    <v-col cols="2">
      <v-img :src="require('../assets/Ellipse 18.png')" height="40"> </v-img>
    </v-col>
    <v-col cols="4"> {{ Username }}</v-col>
    <v-col cols="4" class="d-flex align-center">
      <div class="Eingeloggt mr-2"></div>
      <div>eingeloggt</div>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: "ButtonPrompts",

  props: {
    Username: String,
  },
  data: () => ({}),
};
</script>
<style scoped>
.Eingeloggt {
  width: 5px;
  height: 5px;
  background: #00e817;
  border-radius: 50%;
}
</style>
