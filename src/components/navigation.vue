<template>
    <nav class="main-nav">
    <ul>
      <li>
        <a @click="openModal">Settings</a>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: "navigation",
  props: ["settings"],
  methods: {
    openModal() {
      this.$emit("open", true);
    }
  }
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

  a {
    cursor: pointer;
    transition: 300ms ease-in-out;
    border-bottom: 2px solid transparent;
    &:hover {
      border-color: initial;
      color: rgba(0, 0, 0, 0.25);
    }
  }

  select {
    border: none;
    border-bottom: 2px solid;
    border-radius: 0;
    padding: 0;
    background: transparent;
    -webkit-appearance: none;
    &:focus {
      outline: none;
    }
  }
}
</style>