<template>
  <div>
    <v-img :src="jobID" :key="progress" width="100%" aspect-ratio="1"> </v-img>
  </div>

  <v-row no-gutters justify="space-between" class="ProgressData pa-2">
    <div>{{ remainingTime }}</div>
    <div>{{ progress }}</div>
  </v-row>

  <v-row class="Buttons">
    <v-col>
      <v-btn class="ButtonAbbrechen" block @click="$emit('abbrechen')">abbrechen </v-btn>
    </v-col>
    <v-col v-if="currentJob != undefined">
      <a :href="imageUrl" download target="_blank" class="BildZeigen">
        <v-btn class="ButtonSpeichern" block>bild anzeigen</v-btn>
      </a>
    </v-col>
  </v-row>
  <v-progress-linear
    class="ProgressBar"
    v-model="progress"
    color="white"
  ></v-progress-linear>
</template>
<script>
//http://127.0.0.1:5000/result? job_id=860265c814bfe302197bbd17dc829a206868c02c8daba9745f339fce9bb2bc82 &state=176
const route = "result";
export default {
  data: () => ({
    IPAdresse: "http://172.24.165.75:5000/",
    Progress: 50,
  }),
  computed: {
    jobID() {
      return (
        this.IPAdresse +
        route +
        "?job_id=" +
        this.currentJob?.job_id +
        "&" +
        this.progress
      );
    },
    imageUrl() {
      return this.IPAdresse + route + "/" + this.currentJob?.job_id + ".png";
    },
    progress() {
      if (this.currentJob) {
        return this.currentJob.percent;
      }
      return 0;
    },
    remainingTime() {
      try {
        let seconds = 0;
        if (this.currentJob) {
          seconds = this.currentJob.remaining_time_current;
        }
        return new Date(seconds * 1000).toISOString().slice(11, 19);
      } catch (e) {
        return "-";
      }
    },
  },
  props: {
    currentJob: Object,
  },
};
</script>
<style scoped>
.ProgressBar {
  opacity: 0.42;
}
.ButtonAbbrechen {
  background: #ffdd00;
  color: black;
  border-radius: 20px;
  text-transform: lowercase;
}

.ButtonSpeichern {
  background: white;
  color: black;
  border-radius: 20px;
  text-transform: lowercase;
}
.Buttons {
  position: fixed;
  bottom: 50px;
  z-index: 2;
  left: 20px;
  right: 32px;
}
.BildZeigen {
  display: block;
}
</style>
