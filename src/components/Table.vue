<template>
  <td>
    <table border="1">
      <caption>
        Расписание
      </caption>

      <template v-for="(employee, index) in monthYearToSchedule.get(dateString)" :key="employee.id + index">
        <tr v-if="index === 0">
          <th></th>
          <template v-for="(day, index) in allDaysInMonth" :key="day.getUTCMilliseconds() + index">
            <th v-bind:class="{ weekend: isWeekend(day) }">
              {{ day.toLocaleDateString('ru').slice(0, 5) }}
            </th>
          </template>
          <th class="summary">Всего</th>
          <th class="summary">Ночных</th>
          <th class="summary">Празд.</th>
        </tr>
        <tr>
          <th><input v-model="employee.name"/></th>
          <td
              v-for="(load, index) in employee.schedule"
              :key="employee.name + index"
          >
            <input v-model="load.raw" class="hours" maxlength="3"/>
          </td>
          <td>
            {{ sumHours(employee) }}
          </td>
          <td>
            {{ sumHours(employee, 'н') }}
          </td>
          <td>
            {{ sumHours(employee, 'п') }}
          </td>
        </tr>
      </template>
    </table>
  </td>
  <button v-on:click="addEmployee">Add employee</button>
</template>
<script>
export default {
  computed: {
    allDaysInMonth: function () {
      let tmpDate = new Date(this.date);
      let days = [];
      while (tmpDate.getMonth() === this.date.getMonth()) {
        days.push(new Date(tmpDate));
        tmpDate.setDate(tmpDate.getDate() + 1);
      }
      return days;
    },
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
    isWeekend: function (date) {
      return (!(date.getDay() % 6))
    },
    sumHours: function (employee, token = null) {
      return employee.schedule
          .map((a) => a.raw)
          .filter((a) => parseInt(a))
          .filter((a) => token == null ? true : a.toLowerCase().includes(token))
          .map((a) => parseInt(a))
          .reduce((a, b) => a + b, 0)
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
        schedule.push({raw: null})
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
.hours {
  width: 5ch;
}

.weekend {
  background: tomato;
}

.summary {
  background: lightgray;
}
</style>
