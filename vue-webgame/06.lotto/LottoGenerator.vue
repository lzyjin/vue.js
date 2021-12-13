<template>
  <div>
    <div>당첨 숫자</div>
    <div id="결과창">
      <lotto-ball v-for="ball in winBalls" v-bind:key="ball.idx" number="5"></lotto-ball>
    </div>
    <div>보너스</div>
    <lotto-ball v-if="bonus"></lotto-ball>
    <button v-if="redo">한 번 더!</button>
  </div>
</template>

<script>
// 현재 LottoGenerator 컴포넌트가 부모컴포넌트, LottoBall 컴포넌트가 자식컴포넌트
// 부모컴포넌트에서 자식컴포넌트의 태그부분에 속성처럼 number="5"라고 적으면 
// 자식컴포넌트에서 props속성으로 number을 받아올 수 있다
// props: 자식컴포넌트에게 값을 물려주는 것

// LottoGenerator는 로또당첨숫자를 만드는 곳, LottoBall은 당첨숫자를 화면에 표시하는 역할
// 데이터를 만드는 곳과 데이터를 표시하는 곳이 다르네

import LottoBall from './LottoBall';

function getWinNumbers() {
  console.log('getWinNumbers');
  const candidate = Array(45).fill().map((v, i) => i + 1);
  const shuffle = [];
  while(candidate.length > 0) {
    shuffle.push(candidate.splice(Math.floor(Math.random() * candidate.length), 1)[0]);
  }
  const bonusNumber = shuffle[shuffle.length - 1];
  const winNumbers = shuffle.slice(0, 6).sort((p, c) => p - c);
  return [...winNumbers, bonusNumber];
}

  export default {
    // 자식컴포넌트를 사용하기위해 불러옴
    components: {
      // 'lotto-ball': LottoBall,
      // 축약형
      LottoBall
      // 파스칼케이스를 자동으로 케밥케이스로 바꿔준다 
      // ( 실제 객체이름과 등록할 이름이 일치할 때만 사용 가능 -> 실무에서는 거의 동일하게 사용하므로 축약형으로 쓰자  )
    },
    data() {
      return {
        // winNumbers 와 winBalls 이렇게 두개나 쓰는 이유:
        // 시각적 효과를 위해 winNumbers로 로또번호를 다 뽑아놓고 화면에 하나씩 보여주기 위해
        winNumbers: getWinNumbers(),
        winBalls: [],
        bonus: null,
        redo: false
      }
    },
    computed: {

    },
    methods: {
      
    },
    mounted() {
      for(let i = 0; i < this.winNumbers.length - 1; i++) {
        setTimeout(() => {
          this.winBalls.push(this.winNumbers[i]);
        }, (i + 1) * 1000);
      } 
    },
    beforeDestroy() {
    }
  }
</script>

<style scoped>

</style>
