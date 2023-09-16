<template>
  <div>
    <div>body</div>
    <input type="text" v-model="memo">
    <input type="datetime-local" v-model="date"> <br>
    <button v-on:click="inputValue">추가</button>
    <hr>
    <table id="vue-table">
      <thead>
      <tr>
        <th>MEMO</th>
        <th>DATE</th>
        <th></th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="row in rows" :key="row.id">
        <td>{{ row.memo }}</td>
        <td>{{ row.date }}</td>
        <td><input type="radio" name="header"></td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "AppBody",
  setup() {
    // const rows = reactive([]);

  },
  data: function () {
    return {
      memo: '',
      date: '',
      rows: []
    }
  },
  methods: {
    inputValue: function () {
      this.date = this.date.replace("T", " ");

      this.$data.rows.push({
        id: Math.random(),
        memo: this.memo,
        date: this.date
      });

      console.log(this.$data.rows);
      this.memo = '';
      this.date = '';

    }
  }
}
</script>

<style scoped>
table {
  border: 1px solid black;
}
th, td {
  border: 1px solid black;
}
</style>