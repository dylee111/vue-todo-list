<template>
  <div>
    <input type="text" v-model="memo" v-on:keyup.enter="inputValue">
    <input type="datetime-local" v-model="date"> <br>
    <button v-on:click="inputValue" v-on:keyup.enter="inputValue">추가</button>
    <button v-on:click="deleteMemo">삭제</button>
    <hr>
    <table id="vue-table">
      <thead>
      <tr>
        <th></th>
        <th>MEMO</th>
        <th>DATE</th>
        <th></th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="row in rows" :key="row.todoId" @dblclick="chgMemo(row)" :id="row.todoId" v-on:keyup.enter="modifyMemo(row)">
        <td><input type="radio" name="header" @click="sendMemo(row.todoId)"></td>
        <td v-show="false">{{ row.todoId }}</td>
        <td> <input type="text" v-bind:value="row.todo" readonly class="todoInput"></td>
        <td> <span v-show="isDouble == false">{{ row.toDoDttm }}</span> <input v-show="isDouble == true" type="datetime-local" > </td>
        <td><input type="checkbox" v-model="checkBox" :value="row.todoId"></td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "AppBody",
  setup() {
    // const rows = reactive([]);

  },
  mounted() {
    this.getMemoList();
  },
  data: function () {
    return {
      memo: '',
      date: '',
      mMemo: '',
      mDate: '',
      isDouble: false,
      rows: [],
      checkBox: []
    }
  },
  methods: {
    /* 메모 리스트  */
    getMemoList: function() {
      let main ;
      axios.get('/todo/allList')
      .then(res => {
            console.log(res.data);
            this.rows = res.data;

            this.rows.forEach(function (value) {
              if("1" == value.mainTodo) {
                main = value.todoId;
              }
            });
            this.sendMemo(main);
          })
    },
    /* 메모 저장 */
    inputValue: function () {
      console.log("MEMO SAVE START ======== ");
      let input = [];
      input.push({
        todo: this.memo,
        toDoDttm: this.date
      })

      /* MEMO SAVE */
      axios.post('/todo/saveMemo', JSON.stringify(input), {
            headers: { "Content-Type": "application/json"
        }
      })
          .then((res) => {
            if(res.data == "SUCCESS") {
              this.memo = "";
              this.getMemoList();
            } else {
              alert(res.data);
            }
          });

    },
    /* 메모 삭제 */
    deleteMemo: function () {
      let idList = [];
      this.checkBox.forEach(function (value) {
        console.log(value);
        idList.push(value);
      })
      console.log(idList);
      /* DELETE MEMO */
      axios.post('/todo/deleteMemo', idList)
          .then((res) => {
            this.getMemoList();
          });
    },
    /* 메인 메모 설정 */
    sendMemo: function (memoId) {

      if(null != memoId) {
        axios.put("/todo/mainMemo", memoId, {
          headers: {"Content-Type": "application/json"}
        })
            .then((res) => {
              let memo = res.data.todo;
              let date = res.data.toDoDttm;
          this.$emit("memo", memo, date);

        });
      } else {
        this.$emit("memo", "메모 입력 후 메인 메모 설정", new Date());
      }
    },
    /* 메모 수정 */
    chgMemo: function (row) {
      console.log("ID ::: "  + row.todoId);
      let memoRow = document.getElementById(row.todoId);
      memoRow.children[3].children[1].value = row.toDoDttm.split(".")[0];

      this.isDouble = true;

      memoRow.children[2].children[0].readOnly = false;

    },
    modifyMemo: function (row) {
      let memoRow = document.getElementById(row.todoId);

      let mTodo = memoRow.children[2].children[0].value;
      let mTodoDttm = memoRow.children[3].children[1].value;
      console.log("수정날짜");
      console.log(mTodoDttm);

      memoRow.children[2].children[0].readOnly = true;
      this.isDouble = false;

      let input = {
        id: row.todoId,
        todo: mTodo,
        toDoDttm: mTodoDttm
      }

      if(confirm("메모를 수정하시겠습니까?")) {
        axios.put("/todo/modify", input)
            .then((res) => {
              console.log(res);
              this.getMemoList();
            });
      } else {
        this.getMemoList();
      }
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
.todoInput {
  border: none;
}
.todoInput:focus {
  outline: none;
}
</style>