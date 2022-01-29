<template>
  <td>
    <table border="1">
      <caption>
        Расписание
      </caption>

      <template v-for="(employee, index) in monthYearToSchedule.get(dateString)" :key="employee.id + index">
        <tr v-if="index === 0">
          <th>ФИО</th>
          <th>Должность</th>
          <template v-for="(day, index) in allDaysInMonth" :key="day.getUTCMilliseconds() + index">
            <th v-bind:class="{ weekend: isWeekend(day) }">
              {{ day.toLocaleDateString('ru').slice(0, 2) }}
            </th>
          </template>
          <th class="summary">Всего</th>
          <th class="summary">Ночных</th>
          <th class="summary">Празд.</th>
        </tr>
        <tr>
          <th><textarea v-model="employee.name"/></th>
          <th><textarea v-model="employee.function"/></th>
          <td
              v-for="(load, index) in employee.schedule"
              :key="employee.name + index"
          >
            <input v-model="load.raw" class="hours" maxlength="4"/>
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
  <button v-on:click="addEmployee">Добавить работника</button>
  <button v-on:click="generateCSVString">Скачать</button>
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
          .filter((a) => parseFloat(a))
          .filter((a) => token == null ? true : a.toLowerCase().includes(token))
          .map((a) => parseFloat(a))
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
    generateCSVString: function () {
      let res = '\ufeff' + 'Расписание,' + '\n'
      res += ','
      this.allDaysInMonth.forEach((day) => res += day.toLocaleDateString('ru').slice(0, 2) + ',')
      res += 'Всего, Ночных, Празд.\n'
      let schedule = this.monthYearToSchedule.get(this.dateString)
      schedule?.forEach((employee) => {
        res += (employee.name + ',')
        employee.schedule.forEach((sched) => res += (sched.raw ? sched.raw : 0) + ',')
        res += this.sumHours(employee) + ','
        res += this.sumHours(employee, 'н') + ','
        res += this.sumHours(employee, 'п') + '\n'
      })
      let file = new Blob([res], {type: 'text/csv;charset=UTF-8'});
      let a = document.createElement("a"), url = URL.createObjectURL(file);
      a.href = url;
      a.download = this.date.toLocaleDateString('ru').slice(0, 5) + '.csv'
      document.body.appendChild(a);
      a.click();
      setTimeout(() => {
        document.body.removeChild(a);
        window.URL.revokeObjectURL(url);
      }, 0);
      return res
    }
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
