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
      <tr>
<!--        <td v-for="rows in rows.target"></td>-->
<!--        <td></td>-->

      </tr>
     </tbody>
   </table>
 </div>
</template>

<script>
export default {
  name: "AppBody",
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

      this.rows.push(this.info);
      this.info = {};
      this.memo = '';
      this.date = '';


      const handler = {};

      console.log(this.rows[0]);
      const proxy = new Proxy(this.rows, handler);
      console.log(proxy);

      this.addRow();

    },
    addRow: function () {

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