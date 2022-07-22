<template>
  <v-row class="ma-4">
    <v-col cols="6" v-for="(data, i) in Kuenstler" :key="i">
      <div
        :class="{ ausgewaehlteKuenstler: ausgewaehlteKuenstler.includes(i) }"
        class="KuenstlerRahmen pa-2"
      >
        <v-img
          class="KuenstlerAuswahlBild"
          @click="auswaehlen(i)"
          :src="require('@/assets/Kuenstler/' + data + '.png')"
        >
          <div class="KuenstlerAuswahlName">
            <p>
              {{ data.split(" ")[0] }}
            </p>
            <p>
              {{ data.split(" ")[1] }}
            </p>
          </div>
        </v-img>
      </div>
    </v-col>
  </v-row>
</template>
<script>
export default {
  methods: {
    auswaehlen(i) {
      let neueAuswahl = this.ausgewaehlteKuenstler;
      if (this.ausgewaehlteKuenstler.includes(i)) {
        const pos = this.ausgewaehlteKuenstler.indexOf(i);
        neueAuswahl = this.ausgewaehlteKuenstler.filter((_, index) => index != pos);
      } else {
        neueAuswahl.push(i);
      }

      this.$emit("kuenstlerGewaehlt", neueAuswahl);
    },
  },

  data() {
    return {}; //ausgewaehlteKuenstler.includes(i)
  },
  props: {
    Kuenstler: Array,
    ausgewaehlteKuenstler: Array,
  },
};
</script>
<style scoped>
.KuenstlerAuswahlBild {
  position: relative;
}
.KuenstlerAuswahlName {
  position: absolute;
  bottom: 20px;
  left: 10px;
  font-weight: bold;
  font-size: 20px;
  filter: drop-shadow(0px 3px 10px #00000080);
}
.ausgewaehlteKuenstler {
  color: #fedc00;
  border-style: solid;
  border-color: #fedc00;
  border-radius: 25px;
}
.KuenstlerRahmen {
  transition: all 1s;
}
</style>
