<template>
  <div class="container">
    <v-system-bar color="primary" class="status" dark>
      <v-icon>mdi-information</v-icon>
      <span>{{ statusInformation }}</span>
    </v-system-bar>
    <div class="scanner">
      <vue-qr-reader
        :video-width="160"
        :video-height="160"
        :use-back-camera="true"
        :stop-on-scanned="false"
        @code-scanned="onCodeDetected"
      />
    </div>
    <Activity
      class="result"
      :currentActivity="currentActivity"
      v-if="currentActivity"
    />
    <div class="board">
      <ul class="board__list">
        <li v-for="item in board" :key="item.input">
          <v-chip
            class="ma-2 pa-4 darken-2"
            :color="item.color"
            text-color="white"
          >
            <v-icon left>mdi-label</v-icon>
            {{ item.activity.activity }}
            <v-avatar right :color="item.color" class="darken-4 pa-2">
              {{ item.activity.time }}
            </v-avatar>
          </v-chip>
        </li>
      </ul>
    </div>
    <v-btn class="start" color="primary" @click="toNext">Start de klok</v-btn>
  </div>
</template>

<script>
import VueQrReader from "vue-qr-reader/dist/lib/vue-qr-reader.umd.js";
import io from "socket.io-client";

import Activity from "./Activity.vue";
import activities from "../assets/data/cards.json";


export default {
  name: "Prepare",
  props: {
    board: Array,
    ip: String
  },
  mounted() {
    this.socket = io(`https://${this.ip}`);
    this.socket.on("message", (data) => {
      if (this.currentActivity === '') {
        this.updateStatus("Gelieve eerst een activiteit te scannen");
      } else {
        const input = data.substring(1, data.length - 1);
        if (input === "release") {
          this.board.find(
            (item) => item.input === this.currentBoardInput
          ).activity = this.currentActivity;
          this.updateStatus(
            "Activiteit werd op bord geplaatst, scan volgende of druk op start"
          );
          this.currentActivity = "";
          this.currentQrCode = '';
        } else {
          this.currentBoardInput = input;
          this.updateStatus(
            "Activiteit werd op bord geplaatst, laat los om te bevestigen"
          );
        }
      }
    });
  },
  components: {
    VueQrReader,
    Activity,
  },
  data() {
    return {
      socket: {},
      currentQrCode: "",
      currentActivity: "",
      currentBoardInput: {},
      statusInformation: "Wachten op gescande code"
    };
  },
  methods: {
    toNext: function () {
      this.$emit("toNext", "Progress");
    },
    onCodeDetected: function (code) {
      if (this.currentQrCode !== code) {
        this.currentQrCode = code;
        this.currentActivity = activities.find((a) => a.id === code);
        this.updateStatus("Activiteit werd ingescand, plaats deze op het bord");
      }
    },
    updateStatus: function (status) {
      this.statusInformation = status;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  padding: 0;
  display: grid;
  gap: 0 1rem;
  grid-template-rows: 2rem auto 4rem;
  grid-template-columns: 1fr 2fr 2fr;
  grid-template-areas:
    "status status status"
    "scanner result board"
    "start start start";
}

.status {
  color: white;
  grid-area: status;
}

.scanner {
  grid-area: scanner;
  display: flex;
  justify-content: center;
  align-items: center;
}

.result {
  grid-area: result;
}

.board {
  grid-area: board;
  display: flex;
  justify-content: center;
  align-items: center;
}

.start {
  grid-area: start;
}

.board__list {
  padding: 0;
  width: 100%;
}

.board__list li {
  width: 100%;
}
</style>