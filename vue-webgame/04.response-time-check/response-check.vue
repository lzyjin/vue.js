<template>
  <div>
    <div id="screen" v-bind:class="state">{{message}}</div>
    <!-- v-bind:class="속성명"를 :class=""로 축약할 수 있다 -->
    <!-- <div id="screen" :class="state">{{message}}</div> -->
    <div>
      <div>평균 시간: {{}}</div>
      <button @click="onReset">리셋</button>
    </div>
  </div>
</template>

<script>
  
  export default {
    data() {
      return {
        result: [],
        state: 'waiting',
        // 나는 #screen.waiting #screen.now 이 되길 원한다.
        // 그말은 즉슨 #screen태그가 state요소의 class로 쏙쏙 들어오길 원한다
        // v-bind:class="state"
        message: '클릭해서 시작하세요.'
      }
    }, 
    methods: {
      onReset() {

      }
    }
  }
</script>

// scoped: 현재 컴포넌트에서만 유효한 스타일
<style scoped>
#screen {
  width: 300px;
  height: 200px;
  text-align: center;
  user-select: none;
}
#screen.waiting {
  background-color: aqua;
}
#screen.ready {
  background-color: red;
}
#screen.now {
  background-color: greenyellow;
}
</style>
