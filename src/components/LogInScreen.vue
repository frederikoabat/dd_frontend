<template>
  <v-row class="pa-8 justify-center">
    <v-col cols="6">
      <v-img
        :src="require('../assets/Ellipse 18.png')"
        contain
        height="100"
        class="ProfilbildBeispiel"
      />
    </v-col>

    <v-col cols="12">
      <v-text-field v-model="username" dark> </v-text-field>
    </v-col>
    <v-col cols="12">
      <v-btn class="StartButton mx-auto pa-7" block @click="$emit('username', username)">
        weiter
      </v-btn>
    </v-col>
  </v-row>
</template>
<script>
export default {
  data() {
    return {
      username: "",
    };
  },
  methods: {
    emitValue() {
      console.log(this.username);
    },
  },
};
</script>
<style scoped>
#Username {
}
.ProfilbildBeispiel {
  margin-top: 100px;
}
</style>
