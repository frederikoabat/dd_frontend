<template>
  <v-row class="Buttonprompts" align="center">
    <v-col cols="3">
      <v-img :src="Icon" max-width="40"> </v-img>
    </v-col>

    <v-col cols="8">
      <v-row no-gutters>
        <v-col cols="12" class="ÜberschriftKategorien"> {{ Titel }} </v-col>
        <v-col cols="12" class="BeschreibungKategorien"> {{ Beispiel }}</v-col>
      </v-row>
    </v-col>
    <v-col cols="1" v-if="Arrow == true"> > </v-col>
  </v-row>
</template>
<script>
export default {
  name: "ButtonPrompts",

  props: {
    Titel: String,
    Beispiel: String,
    Icon: String,
    Arrow: Boolean,
  },
  data: () => ({}),
};
</script>
<style>
.Buttonprompts {
  background: #232323;
  border-radius: 10px;
  margin: 0;
}
.ÜberschriftKategorien {
  opacity: 0.5;
  font-size: 10px;
}
.BeschreibungKategorien {
  font-weight: bold;
}
</style>
