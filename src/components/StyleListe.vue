<template>
  <v-row align="center" class="ButtonStilSetting pa-1 mx-5 my-1">
    <v-col cols="3">
      <v-img :src="require('@/assets/' + Bild + '.png')"> </v-img>
    </v-col>
    <v-col cols="7">
      <div>
        <p class="ÜberschriftenStilOptionen">
          {{ Titel }}
        </p>

        <div class="Beispiele">
          {{ Beispiele }}
          <span v-if="Titel == 'Künstler'" class="weitereKuenstler">und weitere</span>
        </div>
      </div>
    </v-col>
    <v-col cols="2" v-if="Pfeil == true"> > </v-col>
  </v-row>
</template>
<script>
export default {
  name: "StyleSettings",

  props: {
    Titel: String,
    Beispiele: String,
    Bild: String,
    Pfeil: Boolean,
  },
  data: () => ({}),
};
</script>
<style scoped>
.ÜberschriftStil {
  font-weight: bold;
}

.ÜberschriftenStilOptionen {
  color: white;
  opacity: 0.5;
  font-size: 10px !important;
}
.ButtonStilSetting {
  background: #232323;
  border-radius: 10x;
  font-size: 16px !important;
  border-radius: 20px;
}
.weitereKuenstler {
  color: white;
  opacity: 0.5;
  font-size: 10px !important;
}
</style>
