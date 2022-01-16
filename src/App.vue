<template>
  <input id="start" v-model="date" min="2020-12" name="start" type="month"/>
  <img alt="Vue logo" src="./assets/logo.png"/>
  <Table :date="dateOnFirstDayOfMonth"/>
</template>
<script>
import Table from "./components/Table.vue";

export default {
  name: "App",
  components: {
    Table,
  },
  data() {
    return {
      date: null
    };
  },
  computed: {
    dateOnFirstDayOfMonth: function () {
      let today = this.date == null ? new Date() : new Date(this.date)
      return new Date(today.getFullYear(), today.getMonth(), 1)
    }
  },

};
</script>

<style>
</style>
