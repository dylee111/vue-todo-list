<template>
<div>
<!--  <app-header v-bind:프로스 속성 이름="상위 컴포넌트의 데이터 이름"></app-header>-->
  <app-header
      v-bind:propsdata="str"
      v-on:renew="renewStr"></app-header>
  <app-body></app-body>
</div>
</template>

<script>
import AppHeader from './components/AppHeader.vue'
import AppBody  from "@/components/AppBody";

export default {
  data: function () {
    return {
      str: "Header!!!!"
    }
  },
  components: {
    AppBody,
    'app-header': AppHeader
  },
  methods: {
    renewStr: function () {
      this.str = 'HI'
    }
  }
}
</script>

<style>

</style>