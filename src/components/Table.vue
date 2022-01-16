<template>
  <td>
    <table border="1">
      <caption>
        Расписание
      </caption>
      <div v-for="(employee, index) in monthYearToSchedule.get(dateString)" :key="employee.id + index">
        <tr>
          <th><input v-model="employee.name"/></th>
          <td
              v-for="(load, index) in employee.schedule"
              :key="employee.name + index"
          >
            <input v-model="load.raw"/>
          </td>
          <td>
            {{ sumHours(employee) }}
          </td>
        </tr>
      </div>
    </table>
  </td>
  <button v-on:click="addEmployee">Add employee</button>
</template>
<script>
export default {
  computed: {
    dateString: function () {
      console.log("dateString " + this.date);
      return this.date.toLocaleDateString();
    },
    daysInMonth: function () {
      return new Date(this.date.getFullYear(), this.date.getMonth() + 1, 0).getDate();
    },
  },
  name: "Table",
  props: {
    date: Date,
  },
  data() {
    return {
      monthYearToSchedule: new Map([[this.dateString, []]]),
    };
  },
  methods: {
    sumHours: function (employee) {
      return employee.schedule
          .map((a) => parseInt(a.raw))
          .reduce((a, b) => a + b, 0);
    },
    initScheduleIfNeeded: function () {
      if (!this.monthYearToSchedule.has(this.dateString)) {
        this.monthYearToSchedule.set(this.dateString, []);
      }
    },
    addEmployee: function () {
      this.initScheduleIfNeeded();
      let schedule = []
      for (let i = 0; i < this.daysInMonth; i++) {
        schedule.push({raw: 0})
      }
      this.monthYearToSchedule.get(this.dateString).push({
        name: "Фамилия",
        schedule: schedule,
        id: Math.floor(Math.random() * 100)
      });
    },

  },
};
</script>
<style>
</style>
