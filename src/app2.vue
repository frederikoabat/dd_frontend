<template>
  <v-app>
    <v-main>
      <HelloWorld @query-job="queryJob(DEMO_JOB)"/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

const DEMO_JOB = {
  args: {
    text_prompts: {
      "0": ["A picture of aa beautiful dasd with rolling hills, green fields, and a river winding through it. The sky would be a deep blue, and there would be fluffy white clouds drifting by. The sun would be shining, and the whole scene would be bathed in a warm, golden light. By Greg Rutkowski, Trending on Artstation."]
    },
    device: "auto"
  }
}


export default {
  name: 'AppTwo',

  components: {
    HelloWorld,
  },

  async created() {
    const serverUrl = 'ws://172.24.165.75:4567/websocket';

    await this.getAuthToken()

    this.socket = new WebSocket(serverUrl);
    this.socket.addEventListener('open', (event) => {
      console.log("event", event)
      this.socket.send(JSON.stringify({
        auth_token: this.token,
      }));
    });

    this.socket.addEventListener('message',  (event) => {
      console.log('Message from server ', event.data);
      try {
        const data = JSON.parse(JSON.parse(event.data))
        console.log('data', typeof data)
        console.log("data.job_id", data["job_id"])
        if (data.job_id) {
          console.log("set current job")
          this.currentJob = data
        }
      } catch (e) {
        console.log('error', e)
      }
    });
  },

  methods: {
    setJob(job) {
      this.currentJob = job
    },
    async getAuthToken() {
      let token = localStorage.getItem('token');
      if (token) {
        this.token = token
        return
      }
      let asyncResponse = await fetch('http://172.24.165.75:5000/socket_auth_token')
      let response = await asyncResponse.json()
      this.token = response.token
      localStorage.setItem('token', this.token)
    },
    queryJob(job) {
      if(this.currentJob) {
        return
      }
      this.socket.send(JSON.stringify({
        job: job
      }));
    }
  },

  data: () => ({
    DEMO_JOB,
    socket: null,
    token: null,
    currentJob: null,
  }),


}
</script>
