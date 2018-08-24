<template>
    <nav class="main-nav">
    <ul>
      <!-- <li>List of Exercises</li> -->
      <li>Settings</li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: "navigation",
  props: ["settings"]
};
</script>

<style scoped lang="scss">
.main-nav {
  text-align: center;
  padding: 20px 0;
  @media (min-width: 992px) {
    position: absolute;
    right: 40px;
    top: 30px;
    text-align: left;
  }

  ul {
    padding: 0;
    margin: 0;
  }

  li {
    display: inline-block;
    padding: 0 10px;
    text-transform: uppercase;
    font-weight: 800;
    letter-spacing: 1px;
  }
}
</style>