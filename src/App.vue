<template>
<div>
<!--  <app-header v-bind:프로스 속성 이름="상위 컴포넌트의 데이터 이름"></app-header>-->
  <app-header
      v-bind:propsdata="str"
      v-on:renew="renewStr"></app-header>
  <app-body @memo=setHeader></app-body>
  <app-sidebar></app-sidebar>
</div>
</template>

<script>
import AppHeader from './components/AppHeader.vue'
import AppBody  from "@/components/AppBody";
import AppSideBar from "@/components/AppSideBar";

export default {
  data: function () {
    return {
      str: " ",
      date: ""
    }
  },
  components: {
    'app-body': AppBody,
    'app-header': AppHeader,
    'app-sidebar': AppSideBar
  },
  methods: {
    renewStr: function () {
      this.str = 'HI'
    },
    setHeader: function (memo, date) {
      this.str = memo;
      this.date = date;
      console.log(memo, date);
    }
  }
}
</script>

<style>

</style>