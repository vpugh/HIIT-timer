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
    },
    updateContent: function(value) {
      this.$emit("input", value);
    },
    closeModal() {
      this.$emit("close", true);
    }
  }
};
</script>

<style scoped lang="scss">
@import "../styles/theme-colors";
@import "../styles/modal";
</style>