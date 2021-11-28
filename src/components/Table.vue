<template>
  <td>
    <table style="width: 100%" border="1">
      <caption>
        Расписание
      </caption>
      <div v-for="(employee, index) in employees" :key="employee.name + index">
        <tr>
          <th>{{ employee.name }}</th>
          <td
            v-for="(load, index) in employee.schedule"
            :key="employee.name + index"
          >
            <input v-model="load.raw" />
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
  name: "Table",
  props: {
    date: Date,
  },
  data() {
    return {
      employees: [
        { name: "Фамилия 1", schedule: [{ raw: 8 }, { raw: 8 }, { raw: 8 }] },
        { name: "Фамилия 2", schedule: [{ raw: 8 }, { raw: 8 }, { raw: 8 }] },
        { name: "Фамилия 3", schedule: [{ raw: 8 }, { raw: 8 }, { raw: 8 }] },
      ],

      schedules: ["Фамилия 1", "Фамилия 2", "Фамилия 3", "Фамилия 4"],

      items: [
        { name: 40, first_name: "Dickerson", last_name: "Macdonald" },
        { name: 21, first_name: "Larsen", last_name: "Shaw" },
      ],
    };
  },
  methods: {
    sumHours: function (employee) {
      return employee.schedule
        .map((a) => parseInt(a.raw))
        .reduce((a, b) => a + b, 0);
    },
    addEmployee: function () {
      console.log(this.date.toString());
      this.employees.push({
        name: "Фамилия 4",
        schedule: [{ raw: 10 }, { raw: 9 }, { raw: 3 }],
      });
    },
  },
};
</script>