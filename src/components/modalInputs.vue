<template>
    <input type="text" :value="value" :name="name" @change="updateContent($event.target.value, $event.target.name)">
</template>

<script>
export default {
  name: "modalInputs",
  props: ["value", "name"],
  methods: {
    updateContent: function(value, name) {
      this.$emit("input", value, name);
      console.log("Modal Inputs " + value, name);
    }
  }
};
</script>

<style scoped>
</style>