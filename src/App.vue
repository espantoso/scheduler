<template>
  <input v-model="date" type="month" id="start" name="start" min="2020-12" />
  <img alt="Vue logo" src="./assets/logo.png" />
  <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
  <Table :date="new Date(date)" />
</template>

<script>
// import HelloWorld from "./components/HelloWorld.vue";
import Table from "./components/Table.vue";

export default {
  name: "App",
  components: {
    // HelloWorld,
    Table,
  },
  data() {
    return {
      date: new Date(),
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
