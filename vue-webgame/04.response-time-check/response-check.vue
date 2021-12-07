<template>
  <div>
    <div id="screen" v-bind:class="state" @click="onClickScreen">{{message}}</div>
    <!-- v-bind:class="속성명"를 :class=""로 축약할 수 있다 -->
    <!-- <div id="screen" :class="state">{{message}}</div> -->
    <div>
      <div>평균 시간: {{  result.reduce( (a, c) => a + c, 0 ) / result.length || 0 }}ms</div>
      <button @click="onReset">리셋</button>
    </div>
  </div>
</template>

<script>
  // 화면이랑 관련 없는 애들은 data에 넣지 않음
  let startTime = 0;
  let endTime = 0;
  let timeout = null;
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
        this.result = [];
      },
      onClickScreen() {
        if(this.state === 'waiting') {

          this.state = 'ready';
          this.message = '초록색이 되면 클릭하세요'
          timeout = setTimeout( () => {
            this.state = 'now';
            this.message = '지금 클릭!'
            startTime = new Date();
          }, Math.floor( Math.random() * 1000 ) + 2000 ); // 2 ~ 3 초

        } else if(this.state === 'ready') {

          clearTimeout(timeout);
          this.state = 'waiting';
          this.message = '너무 성급하시군요.! 초록색이 된 후에 클릭하세요'

        } else if(this.state === 'now') {

          endTime = new Date();
          this.state = 'waiting';
          this.message = '클릭해서 시작하세요';
          this.result.push(endTime - startTime);

        }
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
