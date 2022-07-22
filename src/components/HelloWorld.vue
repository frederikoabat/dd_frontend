<template>
  <v-container>
    <v-row justify=" start" class="pa-10">
      <v-col cols="12" class="mb-n9 ml-n5 mt-10">
        <v-img
          :src="require('../assets/logo_big.svg')"
          contain
          height="100"
          width=" 100"
        />
      </v-col>

      <v-col class="StartText" cols="12">
        <h1 class="ÜberschriftDiscoDiffusion">Disco Diffusion</h1>
        <p class="EinführungText">
          Der Disco Diffusion Algorithmus ist eine Abwandlung von OpenAI's Dall-e 2. Er
          generiert aus Text Bilder in einem durch CLIP (Sprachmodel) geführten
          Diffusionsprozess (Bildsynthese).
        </p>
        <a class="LinkmehrErfahren" href="https://community.vuetifyjs.com" target="_blank"
          >mehr erfahren</a
        >
      </v-col>
      <v-col>
        <v-btn
          class="ButtonGrau mx-auto mb-3 pa-6"
          :color="colour"
          @click="changeColour"
          href="https://colab.research.google.com/github/alembics/disco-diffusion/blob/main/Disco_Diffusion.ipynb"
          block
        >
          Colab Notebook
        </v-btn>

        <v-btn class="StartButton mx-auto mb-3 pa-6" block @click="$emit('next')">
          los geht's
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",

  data: () => ({
    colour: "#232323",
    ecosystem: [
      {
        text: "vuetify-loader",
        href: "https://github.com/vuetifyjs/vuetify-loader",
      },
      {
        text: "github",
        href: "https://github.com/vuetifyjs/vuetify",
      },
      {
        text: "awesome-vuetify",
        href: "https://github.com/vuetifyjs/awesome-vuetify",
      },
    ],
    importantLinks: [
      {
        text: "Chat",
        href: "https://community.vuetifyjs.com",
      },
      {
        text: "Made with Vuetify",
        href: "https://madewithvuejs.com/vuetify",
      },
      {
        text: "Twitter",
        href: "https://twitter.com/vuetifyjs",
      },
      {
        text: "Articles",
        href: "https://medium.com/vuetify",
      },
    ],
  }),
};
</script>
<style scoped>
.EinführungText {
  text-align: left;
  color: white;
  opacity: 0.5;
}
.LinkmehrErfahren {
  color: white !important;
}
</style>
