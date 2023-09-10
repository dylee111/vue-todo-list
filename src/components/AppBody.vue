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
      </tr>
      </thead>
      <tbody>
      <tr >


      </tr>
      </tbody>
    </table>
    <div >
      {{ rows }}
      <hr>
      {{ info }}
      <br>
    </div>
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
      rows: [],
      info: {
        memo: '',
        date: ''
      }
    }
  },
  methods: {
    inputValue: function () {
      this.date = this.date.replace("T", " ");
      this.info.memo = this.memo;
      this.info.date = this.date;

      let tmpJson = JSON.parse(JSON.stringify(this.info));
      this.$data.rows.push(tmpJson);
      let testJson = JSON.parse(JSON.stringify(this.$data.rows));

      testJson.forEach(function (row) {
        console.log("log");
        console.log(row.memo);
      })
      console.log(testJson);
      this.info = {};
      this.memo = '';
      this.date = '';

      console.log("----1111");
      // console.log(handler.get(this.$data.rows, this.$data.rows[0]));

    },
    jsonConvert: function (obj) {
      obj = JSON.parse(JSON.stringify(obj))
      return obj;
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