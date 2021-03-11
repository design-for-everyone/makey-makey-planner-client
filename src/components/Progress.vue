<template>
  <div class="container">
    <Activity
      v-if="board[activityIndex].activity.time"
      class="activity"
      :currentActivity="board[activityIndex].activity"
    />
    <div class="activity__progress">
      <v-progress-circular
        rotate="-90"
        size="220"
        width="35"
        :value="activityTime"
        :color="board[activityIndex].color"
      ></v-progress-circular>
    </div>
    <div class="total__progress">
      <v-progress-linear
        v-for="(item, index) in board"
        class="progressbar"
        :key="item.input"
        :style="{ width: board[index].barWidth + '%' }"
        height="35"
        :value="
          index === activityIndex
            ? activityTime
            : index < activityIndex
            ? 100
            : 0
        "
        background-color="lighten-4"
        :color="board[index].color"
      ></v-progress-linear>
    </div>
  </div>
</template>

<script>
import io from "socket.io-client";
import Activity from "./Activity.vue";

export default {
  name: "Progress",
  props: {
    board: Array,
    ip: String,
  },
  components: {
    Activity,
  },
  mounted() {
    // watch if an activity is checked
    this.socket = io(`https://${this.ip}`);
    this.socket.on("message", (data) => {
      const input = data.substring(1, data.length - 1);
      if (input !== "release") {
        const boardItem = this.board.find((item) => item.input === input);
        console.log(boardItem);
        boardItem.completed = true;
      }
    });

    // calculate total time
    const totalTime = this.board
      .map((item) => (item.activity.time ? item.activity.time : 0))
      .reduce((acummulator, currentValue) => acummulator + currentValue);

    // set a percentage for each activity on the board
    this.board.forEach((item) => {
      item.barWidth = parseInt((item.activity.time / totalTime) * 100);
      if (isNaN(item.barWidth)) {
        item.barWidth = 0;
        item.completed = true;
      }
    });

    // start the timer
     this.startTimer();
  },
  data() {
    return {
      socket: {},
      activityIndex: 0,
      activityTime: 0,
      interval: {},
    };
  },
  methods: {
    startTimer: function () {
      // quit the component if all activities has been checked
      if (this.activityIndex === this.board.length) {
        this.toNext();
      } else {
        const boardItem = this.board[this.activityIndex];
        // go to next activity if an activity is completed
        if (boardItem.completed) {
          // check if it was marked as completed by the user
          if (boardItem.barWidth > 0) {
            this.activityTime = 100;
          }
          this.activityIndex++;
          this.startTimer();
        } else {
          // calculate the step for each iteration in the activity (divide by 10 for interval of 100ms)
          const intervalPercentage =
            100 / (this.board[this.activityIndex].activity.time * 60) / 10;

          this.interval = setInterval(() => {
            // check if activity is completed by time
            if (this.activityTime < 100) {
              // no: update time
              this.activityTime += intervalPercentage;
              // check if activity is completed by user
              if(boardItem.completed && boardItem.barWidth > 0){
                this.activityTime = 100;
              }
            } else {
              // yes: stop the timer and go the the next activity
              this.nextActivity();
            }
          }, 100);
        }
      }
    },
    nextActivity: function () {
      clearInterval(this.interval);
      this.activityTime = 0;
      this.activityIndex += 1;
      this.startTimer();
    },
    toNext: function () {
      this.$emit("toNext", "Finished");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: grid;
  grid-template-rows: 75% 25%;
  grid-template-columns: 1fr 1fr;
  grid-template-areas:
    "activity progress"
    "total total";
}

.activity {
  grid-area: activity;
}

.activity__progress {
  grid-area: progress;
  display: flex;
  justify-content: center;
  align-items: center;
}

.total__progress {
  grid-area: total;
  display: flex;
  align-items: center;
}

.progressbar {
  border: 0.1rem solid lightgrey;
  border-left-width: 0;
  padding: 0;
}
</style>