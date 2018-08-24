<template>
  <div id="app">
  <navigation @click.native="settingModal = !settingModal"></navigation>
  <modal :settingModal="this.settingModal" :stages="this.stages" :round="rounds" :exercise="amtExercise" :work="workTime" :rest="restTime" @close="closeModal" @input="handleInputChange"></modal>
  <div class="main">
    <div class="stage" v-html="currentStage"></div>
    <div class="state" v-html="currentState"></div>
    <div class="timer" v-html="workTotal"></div>
    <div class="buttons">
      <button @click="timerPlay" v-if="!timerRunning">Start <span class="fa fa-play"></span></button>
      <button @click="timerPause" v-if="timerRunning">Pause <span class="fa fa-pause"></span></button>
      <button @click="timerReset">Restart <span class="fa fa-redo"></span></button>
    </div>
  </div>
  </div>
</template>

<script>
import navigation from "./components/navigation.vue";
import modal from "./components/modal.vue";
export default {
  name: "app",
  components: {
    navigation,
    modal
  },
  data() {
    return {
      rounds: 3,
      amtExercise: 4,
      workTime: 20,
      restTime: 10,
      totalWorkTime: this.workTotal,
      totalRestTime: "",
      timerRunning: false,
      interval: null,
      settingModal: false,
      round: 0,
      stages: [],
      exercise: []
    };
  },
  created() {
    this.totalWorkTime = this.workTime;
    this.createStages();
    this.createExercise();
  },
  computed: {
    workTotal: function() {
      var time = this.totalWorkTime;
      var sec = time >= 10 ? time : "0" + time;
      return ":" + sec;
    },
    totalRounds: function() {
      var combinedStage = this.amtExercise * 2;
      return combinedStage * this.rounds - 1;
    },
    currentStage: function() {
      var stage = this.stages,
        circuit = Math.ceil((this.round + 1) / (this.amtExercise * 2));
      return "Round " + circuit + "/" + this.rounds;
    },
    currentState: function() {
      var round = this.round,
        position = this.exercise[round] + 1,
        circuit = Math.ceil((this.round + 1) / (this.amtExercise * 2)),
        truePosition =
          this.exercise[round] + 1 <= this.amtExercise * 2
            ? position
            : this.exercise[round - this.amtExercise * 2 * (circuit - 1) + 1];
      if (position % 2 == 0) {
        return "Rest";
      } else {
        return "Exercise";
      }
    }
  },
  methods: {
    closeModal() {
      if ("close" == false) {
      } else {
        this.settingModal = !this.settingModal;
      }
    },
    handleInputChange(value, name) {
      if (name === "workTime") {
        this.workTime = value;
        this.updateTotalWork();
      } else if (name === "restTime") {
        this.restTime = value;
        this.updateTotalRest();
      } else if (name === "amtExercise") {
        this.amtExercise = value;
        this.createExercise();
      } else {
        this.rounds = value;
        this.createStages();
      }
    },
    createStages: function() {
      this.stages = [];
      for (var i = 0; i < this.rounds; i++) {
        this.stages.push(i);
      }
    },
    createExercise: function() {
      this.exercise = [];
      for (var i = 0; i < this.amtExercise * 2; i++) {
        this.exercise.push(i);
      }
    },
    updateTotalWork: function() {
      return (this.totalWorkTime = this.workTime);
    },
    updateTotalRest: function() {
      return (this.totalRestTime = this.restTime);
    },
    isEven: function(num) {
      return num % 2 == 0;
    },
    timerPlay() {
      this.timerRunning = true;
      this.timerPaused = false;
      this.interval = setInterval(this.countdownTimer, 1000);
    },
    timerPause() {
      this.timerRunning = false;
      this.timerPaused = true;
      clearInterval(this.interval);
    },
    timerReset() {
      this.timerRunning = false;
      this.timerPaused = false;
      clearInterval(this.interval);
      this.totalWorkTime = this.workTime;
      this.round = 0;
    },
    countdownTimer() {
      var snd = new Audio(
        "http://soundbible.com/mp3/Elevator Ding-SoundBible.com-685385892.mp3"
      );
      var beep = new Audio(
        "http://soundbible.com/mp3/Checkout%20Scanner%20Beep-SoundBible.com-593325210.mp3"
      );
      if (this.totalWorkTime == 0 && this.totalRounds == this.round) {
        this.timerRunning = false;
        this.timerPaused = false;
        clearInterval(this.interval);
        this.roundStep();
      } else if (this.totalWorkTime == 0 && this.totalRounds != this.round) {
        snd.play();
        clearInterval(this.interval);
        this.interval = setInterval(this.countdownTimer, 1000);
        this.roundStep();
      } else if (this.totalWorkTime <= 6 && this.totalWorkTime != 0) {
        beep.play();
        clearInterval(this.interval);
        this.interval = setInterval(this.countdownTimer, 1000);
        this.totalWorkTime--;
      } else {
        this.totalWorkTime--;
      }
    },
    roundStep: function() {
      this.round++;
      var airhorn = new Audio(
        "http://soundbible.com/mp3/Air%20Horn-SoundBible.com-964603082.mp3"
      );
      if (this.round == this.totalRounds) {
        this.timerReset();
        setTimeout(function() {
          airhorn.play();
        }, 1000);
      } else if (this.isEven(this.round) == true) {
        this.totalWorkTime = this.workTime;
      } else if (this.isEven(this.round) == false) {
        this.totalWorkTime = this.restTime;
      }
    },
    activeStage: function(index) {
      return this.round === index;
    }
  }
};
</script>

<style lang="scss">
@import "styles/reset";
@import url("https://fonts.googleapis.com/css?family=Nunito:400,600,700");
@import "styles/theme-colors";

html {
  height: 100%;
}

body {
  background-color: $primary-color;
  background-image: $gradient;
  height: 100%;
  background-repeat: no-repeat;
  background-attachment: fixed;
}

#app {
  font-family: "Nunito", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100vh;
  width: 100vw;
  top: 0;
  left: 0;

  > .main {
    width: 90%;
    box-shadow: 2px 3px 15px rgba(0, 0, 0, 0.4),
      4px 8px 45px 6px rgba(0, 0, 0, 0.2);
    background: #fff;
    border-radius: 4px;
    padding: 30px 0;
    margin: 10px auto 40px auto;
    text-align: center;

    @media (min-width: 992px) {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 50%;
      padding: 60px 0;
      margin: 0 auto;
    }
  }

  .main button {
    background-color: $primary-color;
    color: darken($primary-color, 40%);
    padding: 15px 34px;
    border-radius: 30px;
    border: none;
    margin: 0 10px;
    letter-spacing: 2px;
    text-transform: uppercase;
    transition: 300ms ease-in-out;
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4);

    @media (max-width: 991px) {
      margin-bottom: 20px;
    }

    &:hover {
      background-color: lighten($primary-color, 15%);
      box-shadow: 3px 6px 12px rgba(0, 0, 0, 0.2);
      transform: translatey(-2px);
    }

    &:active,
    &:focus {
      outline: none;
    }

    .fa {
      display: none;
      visibility: hidden;
      @media (min-width: 992px) {
        display: inline-block;
        visibility: visible;
      }
    }
  }
}

@import "styles/timer";
</style>