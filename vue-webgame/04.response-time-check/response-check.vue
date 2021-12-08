<template>
  <!-- 가장 바깥의 template태그 바로 아래에 template을 쓸 수 없다. 그대신 그 아래부터는 몇번이고 사용 가능. -->
  <div>
    <!-- v-bind:class="속성명"를 :class=""로 축약할 수 있다 -->
    <!-- <div id="screen" :class="state">{{message}}</div> -->
    <div id="screen" v-bind:class="state" @click="onClickScreen">{{message}}</div>

    <!-- v-show="" 따옴표안에 들어있는것이 true이면 show(보여주고), falsy이면 숨긴다(태그는 존재하고, display:none) -->
    <!-- v-if와의 차이점은 v-if를 사용하면 태그 자체가 존재하지 않음 -->
    <!-- <div v-show="result.length"> -->
    <!-- <div v-if="result.length"> -->
    <!-- 이 태그는 그냥 평균시간과 리셋버튼을 감싸는 역할만 하기때문에 div로 묶기 싫다면 template으로 바꿔도 된다 -->
    <!-- 그러면 #screen과 평균시간, 리셋버튼은 형제레벨이 된다(템플릿은 없는 태그가 된다) -->
    <template v-if="result.length">

      <!-- 계산은 화면에서 안하는 것이 좋다. vue의 computed를 이용하자 -->
      <!-- <div>평균 시간: {{  result.reduce( (a, c) => a + c, 0 ) / result.length || 0 }}ms</div> -->
      <div>평균 시간: {{ average }}ms</div>
      <button @click="onReset">리셋</button>

    </template>
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
    // computed는 data를 가공해서 사용할 때 사용. computed를 통해 가공하면 캐싱이 된다. 
    // 캐싱이 중요한 이유는? 
    // 일반 데이터인 message가 바뀌면 화면 전체 (template)를 다시 그린다. 그러면 result를 계산하는 부분까지 다시 계산한다. 
    // message만 바뀌었는데 평균시간까지 다시 계산하니까, 예를들어 더 복잡한 계산을 하는 식이라면 메세지만 바꿨는데 로딩하는데 10초 이상이 걸릴 수도 있는 것. 
    // computed를 사용하면 message가 바뀌면 average에는 아무런 영향이 없다.
    // 성능 최적화를 위해 꼭 챙겨야할 부분이다.
    computed: {
      average() {
        return this.result.reduce( (a, c) => a + c, 0 ) / this.result.length || 0 ;
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
