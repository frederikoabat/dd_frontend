<template>
  <v-app class="App">
    <v-main>
      <ToolbarDiscoDiffusion
        v-if="AktuelleSeite != 'Seite1' && AktuelleSeite != 'Seite2'"
        :Username="username"
        @ZurueckZurStartseite="AktuelleSeite = 'Seite1'"
      />
      <HelloWorld @next="AktuelleSeite = 'Seite2'" v-if="AktuelleSeite == 'Seite1'" />
      <LogInScreen v-if="AktuelleSeite == 'Seite2'" @username="OnNextPage" />
      <PictureProgress
        v-if="AktuelleSeite == 'Seite5'"
        :currentJob="currentJob"
        @abbrechen="JobAbbrechen"
      >
      </PictureProgress>
      <PromptSettings
        v-if="AktuelleSeite == 'Seite3' || AktuelleSeite == 'Seite5'"
        :Username="username"
        :Kuenstler="Kuenstler"
        :ausgewaehlteKuenstler="ausgewaehlteKuenstler"
        @next="AktuelleSeite = 'Seite4'"
        @Diffuse="StartDiffuse"
        :DiffuseMode="AktuelleSeite == 'Seite5'"
        @promptChanged="promptChanged"
        :promptText="promptText"
      />
      <StyleSettings
        v-if="AktuelleSeite == 'Seite4'"
        @zurueckZuPromptSettings="SetSettings"
        :Kuenstler="Kuenstler"
        :ausgewaehlteKuenstler="ausgewaehlteKuenstler"
        @kuenstlerGewaehlt="ausgewaehlteKuenstler = $event"
      >
      </StyleSettings>
    </v-main>
  </v-app>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import LogInScreen from "./components/LogInScreen.vue";
import HelloWorld from "./components/HelloWorld.vue";
import PromptSettings from "./components/PromptSettings.vue";
import StyleSettings from "./components/StyleSettings.vue";
import ToolbarDiscoDiffusion from "./components/ToolbarDiscoDiffusion.vue";
import PictureProgress from "./components/PictureProgress.vue";

export default {
  name: "App",
  methods: {
    promptChanged(event) {
      this.promptText = event;
      console.log(event);
    },
    JobAbbrechen() {
      this.cancelJob();
      this.currentJob = null;
      this.AktuelleSeite = "Seite3";
    },
    StartDiffuse(Prompt) {
      let ausgewaehlteKuenstlernamen = [];
      for (let i = 0; i < this.ausgewaehlteKuenstler.length; i++) {
        let aktuellerKuenstlerIndex = this.ausgewaehlteKuenstler[i];
        let aktuellerKuenstlerName = this.Kuenstler[aktuellerKuenstlerIndex];
        ausgewaehlteKuenstlernamen.push(aktuellerKuenstlerName);
      }

      this.prompt = Prompt + " by " + ausgewaehlteKuenstlernamen.join(" and ");
      this.queryJobFromPrompt();
      this.AktuelleSeite = "Seite5";
    },

    OnNextPage(username) {
      this.username = username;
      this.AktuelleSeite = "Seite3";
    },
    SetSettings(gewaehlteEinstellungen) {
      console.log(gewaehlteEinstellungen);
      this.AktuelleSeite = "Seite3";
    },
    setJob(job) {
      this.currentJob = job;
    },
    async getAuthToken() {
      let token = localStorage.getItem("token");
      if (token) {
        this.token = token;
        return;
      }
      let asyncResponse = await fetch("http://172.24.165.75:5000/socket_auth_token");
      let response = await asyncResponse.json();
      this.token = response.token;
      localStorage.setItem("token", this.token);
    },
    queryJob(job) {
      if (this.currentJob) {
        return;
      }
      this.socket.send(
        JSON.stringify({
          job: job,
        })
      );
    },
    cancelJob() {
      if (!this.currentJob) {
        return;
      }
      this.socket.send(
        JSON.stringify({
          req: { cancel: this.currentJob.job_id },
        })
      );
    },
    queryJobFromPrompt() {
      let job = {
        args: {
          text_prompts: {
            0: [this.prompt],
          },
          device: "auto",
          steps: 40,
        },
      };
      this.queryJob(job);
    },
  },

  components: {
    //HelloWorld,
    LogInScreen,
    HelloWorld,
    PromptSettings,
    StyleSettings,
    ToolbarDiscoDiffusion,
    PictureProgress,
  },

  watch: {
    username(newval) {
      console.log(newval);
    },
  },
  async created() {
    const serverUrl = "ws://172.24.165.75:4567/websocket";

    await this.getAuthToken();

    this.socket = new WebSocket(serverUrl);
    this.socket.addEventListener("open", (event) => {
      console.log("event", event);
      this.socket.send(
        JSON.stringify({
          auth_token: this.token,
        })
      );
    });

    this.socket.addEventListener("message", (event) => {
      console.log("Message from server ", event.data);
      try {
        const data = JSON.parse(JSON.parse(event.data));
        console.log("data", typeof data);
        console.log("data.job_id", data["job_id"]);
        if (data.job_id) {
          console.log("set current job");
          this.currentJob = data;
        }
      } catch (e) {
        console.log("-");
      }
    });
  },

  data: () => ({
    promptText: "",
    socket: null,
    token: null,
    currentJob: null,
    prompt: "",
    username: null,
    WeiterButtonGedrückt: false,
    AktuelleSeite: "Seite1",

    Typ: "Painting",
    Stimmung: "beautiful",

    Kuenstler: [
      "Alberth Bierstadt",
      "Alena Aenami",
      "Andrian Luchian",
      "Christophe Young",
      "Cycle Circle",
      "Greg Rutowski",
      "Julain Bauer",
      "Laurie Greasley",
      "Linxin Yin",
      "Matt Sanz",
      "Mike Winkelma",
      "Ross Tran",
      "Thomas Kinkade",
      "Yasar Vurdem",
    ],
    ausgewaehlteKuenstler: [4, 10],
  }),
};
</script>

<style>
.ButtonGrau {
  color: white !important;
  border-radius: 25px;
  text-transform: none !important;
  max-width: 252px !important;

  min-width: auto;
  width: 100%;
}
.App {
  background: black;
  color: white;
}
.StartButton {
  max-width: 252px !important;
  background: #ffdd00;
  border-radius: 25px;
  text-transform: lowercase;
  color: black !important;
  min-width: auto;
  width: 100%;
}
</style>
