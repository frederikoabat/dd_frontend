<template>
  <v-container>
    <v-row align="center">
      <v-col align="center" cols="12" v-if="DiffuseMode == false">
        <div class="NeuesKlein">neues</div>
        <div class="BildGroß">Bild</div>
      </v-col>

      <v-col align="center" cols="12">
        <v-textarea
          :disabled="DiffuseMode == true"
          v-model="Prompt"
          @blur="$emit('promptChanged', Prompt)"
        >
        </v-textarea>
      </v-col>

      <v-col cols="12">
        <ButtonPrompts
          Titel="Stil"
          :Beispiel="Kuenstler[ausgewaehlteKuenstler[0]]"
          :Icon="require('../assets/icons8-illustrator.svg')"
          :Arrow="true"
          @click="KuenstlerAnzeigen"
        >
        </ButtonPrompts>
      </v-col>
      <v-col cols="6">
        <ButtonPrompts
          Titel="Auflösung"
          Beispiel="512*512"
          :Icon="require('../assets/icons8-crop.svg')"
        >
        </ButtonPrompts>
      </v-col>

      <v-col cols="6">
        <ButtonPrompts
          Titel="Engine"
          Beispiel="Auto"
          :Icon="require('../assets/icons8-processor.svg')"
        >
        </ButtonPrompts
      ></v-col>

      <v-col cols="6">
        <ButtonPrompts
          Titel="Bild"
          Beispiel="wat_da"
          :Icon="require('../assets/icons8-image.svg')"
        >
        </ButtonPrompts>
      </v-col>

      <v-col cols="6">
        <ButtonPrompts
          Titel="Schnitte"
          Beispiel="250 skip 100"
          :Icon="require('../assets/icons8-step.svg')"
        >
        </ButtonPrompts>
      </v-col>

      <v-col cols="6">
        <ButtonPrompts
          Titel="CLIP"
          Beispiel="guiddance"
          :Icon="require('../assets/icons8-brain.svg')"
        >
        </ButtonPrompts>
      </v-col>

      <v-col cols="6">
        <ButtonPrompts
          Titel="Symmetrie"
          Beispiel="keine"
          :Icon="require('../assets/icon_symmetry.svg')"
        >
        </ButtonPrompts
      ></v-col>

      <v-col align="center" cols="12" v-if="DiffuseMode == false">
        <v-btn class="StartButton pa-6" block @click="$emit('Diffuse', Prompt)">
          diffuse!
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import ButtonPrompts from "./ButtonPrompts.vue";

export default {
  data: () => ({
    Prompt: "",
  }),
  methods: {
    KuenstlerAnzeigen() {
      if (this.DiffuseMode == false) {
        this.$emit("next");
      } else {
        alert("Bild wird generiert!");
      }
    },
  },

  props: {
    Username: String,
    DiffuseMode: Boolean,
    Kuenstler: Array,
    ausgewaehlteKuenstler: Array,
    promptText: String,
  },
  components: {
    ButtonPrompts,
  },
  created() {
    this.Prompt = this.promptText;
  },
};
</script>
<style scoped>
.eigenerButton {
  background: #232323;
  border-radius: 10px;
  padding: 10px;
}

.grauerText {
  opacity: 0.5;
  color: white;
}
.neuesKlein {
  font-size: 15px !important;
  text-transform: lowercase !important;
}
.BildGroß {
  font-weight: bold;
  font-size: 25px;
}
</style>
