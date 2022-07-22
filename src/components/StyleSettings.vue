<template>
  <v-row justify="center">
    <v-col cols="12" class="ÜberschriftStil ml-5 d-flex" @click="BeiZurueckKlick">
      <v-row no-gutters align="center">
        <p class="StilUeberschrift d-inline-flex ml-2">&lt; Stil</p>

        <img
          class="d-inline-flex ml-2"
          :src="require('../assets/icons8-illustrator.svg')"
          contain
          width="25"
          height="25"
        />
      </v-row>
    </v-col>

    <v-col cols="8"> </v-col>
    <v-row v-if="Unterseite == 'UnterseitenAuswahl'">
      <v-col cols="12">
        <StyleListe
          Titel="Künstler"
          :Beispiele="Kuenstler[ausgewaehlteKuenstler[0]]"
          :Bild="'Kuenstler/' + Kuenstler[ausgewaehlteKuenstler[0]]"
          Pfeil
          @click="Unterseite = 'UnterseiteKuenstler'"
        >
        </StyleListe>
      </v-col>
      <v-col cols="12 ">
        <StyleListe Titel="Typ" Beispiele="Gemälde" Bild="Kuenstler/Alena Aenami" Pfeil />
      </v-col>
      <v-col cols="12">
        <StyleListe Titel="Stimmung" Beispiele="anmutig" Bild="icons8-neutral@3x" Pfeil />
      </v-col>
    </v-row>
    <KuenstlerAuswahl
      v-if="Unterseite == 'UnterseiteKuenstler'"
      :Kuenstler="Kuenstler"
      :ausgewaehlteKuenstler="ausgewaehlteKuenstler"
      @kuenstlerGewaehlt="$emit('kuenstlerGewaehlt', $event)"
    >
    </KuenstlerAuswahl>
  </v-row>
</template>

<script>
import StyleListe from "./StyleListe.vue";
import KuenstlerAuswahl from "./KuenstlerAuswahl.vue";
export default {
  data() {
    return { Unterseite: "UnterseitenAuswahl" };
  },
  props: {
    Titel: String,
    Beispiele: String,
    Bild: String,
    Kuenstler: Array,
    ausgewaehlteKuenstler: Array,
  },
  components: {
    StyleListe,
    KuenstlerAuswahl,
  },
  computed: {
    Einstellungen() {
      return {
        Kuenstler: this.Kuenstler,
        Typ: "Gemaelde",
        Stimmung: "anmutig",
      };
    },
  },
  methods: {
    BeiZurueckKlick() {
      if (this.Unterseite == "UnterseitenAuswahl") {
        this.$emit("zurueckZuPromptSettings", this.Einstellungen);
      } else {
        this.Unterseite = "UnterseitenAuswahl";
      }
    },
  },
};
</script>
<style>
.StilUeberschrift {
  font-size: 25px;
}
</style>
