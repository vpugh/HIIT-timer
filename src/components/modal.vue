<template>
    <transition name="fade">
        <div class="setting-menu" v-if="settingModal === true">
            <div class="init">
                <button @click="closeModal" class="menu-trigger">
                    <span class="cd-icon"></span>
                </button>
                <div>
                    <modalInputs @input="updateValues" type="text" name="rounds" v-model="rounds"></modalInputs>
                    <p>Total: Number of rounds</p>
                </div>
                <div>
                    <modalInputs @input="updateValues" type="text" v-model="amtExercise" name="amtExercise"></modalInputs>
                    <p>Total: Different exercises</p>
                </div>
                <div>
                    <modalInputs @input="updateValues" v-model="workTime" name="workTime"></modalInputs>
                    <p>Workout time for each exercise</p>
                </div>
                <div>
                    <modalInputs @input="updateValues" type="text" v-model="restTime" name="restTime"></modalInputs>
                    <p>Rest period between each exercise</p>
                </div>
            </div>
            <div class="modal-bg"></div>
        </div>
  </transition>
</template>

<script>
import modalInputs from "./modalInputs.vue";
export default {
  name: "modal",
  components: {
    modalInputs
  },
  props: [
    "settingModal",
    "stages",
    "round",
    "exercise",
    "work",
    "rest",
    "value"
  ],
  data() {
    return {
      workTime: this.work,
      restTime: this.rest,
      amtExercise: this.exercise,
      rounds: this.round
    };
  },
  methods: {
    updateValues: function(value, name) {
      this.$emit("input", value, name);
      console.log("Modal " + value, name);
    },
    updateContent: function(value) {
      this.$emit("input", value);
      console.log(value);
    },
    closeModal() {
      this.$emit("close", true);
    }
  }
};
</script>

<style scoped lang="scss">
@import "../styles/theme-colors";
// Modal
.modal-bg {
  position: fixed;
  height: 100vh;
  width: 100vw;
  background-color: rgba(0, 0, 0, 0.75);
  top: 0;
  left: 0;
  z-index: 1;
}
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}

// Menu
.setting-menu {
  height: 100vh;
  width: 100vw;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 2;
}
.init {
  width: 90%;
  margin: 20px auto;
  padding: 20px 0;
  border: 10px solid $primary-color;
  border-radius: 6px;
  display: flex;
  flex-direction: column;
  background-color: #fff;
  z-index: 3;

  @media (min-width: 992px) {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 50%;
    padding: 50px 0;
    margin: 0 auto;
  }

  div {
    display: flex;
    width: 80%;
    margin: 20px auto 0 auto;
    text-align: left;
  }

  p {
    display: block;
  }

  input {
    width: 40%;
    margin-right: 20px;
    border: none;
    border-bottom: 3px solid $primary-color;
    font-size: 22px;
    padding: 0;
    font-weight: bold;
    text-align: center;

    @media (min-width: 992px) {
      width: 10%;
    }
  }
}
.menu-trigger {
  background-color: $accent-color;
  border: 1px solid #eaeaea;
  border-radius: 100px;
  display: block;
  height: 40px;
  width: 40px;
  left: 81%;
  top: 40px;
  position: absolute;
  z-index: 5;

  @media (min-width: 992px) {
    left: 91%;
    top: 20px;
  }

  .cd-icon {
    position: absolute;
    left: 25%;
    top: 47%;
    bottom: auto;
    right: auto;
    display: inline-block;
    background-color: rgba(0, 0, 0, 0);
    height: 4px;
    width: 20px;

    &::before,
    &::after {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      content: "";
      background-color: #4a4a4a;
    }

    &::before {
      transform: rotate(135deg);
    }
    &::after {
      transform: rotate(-135deg);
    }
  }
}
</style>