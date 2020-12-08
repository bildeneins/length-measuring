<template>
  <div id="app">
    <HelloWorld />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
};
</script>

<style>
/* html,
body {
  height: 100%;
} */
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  padding: 60px;
  height: 100%;

  /* ダークモード */
  background-color: #313852;
}

/* .btn-primary{
  color: #fff;
  background-color: #007bff;
  border-color: #007bff;
} */
</style>
