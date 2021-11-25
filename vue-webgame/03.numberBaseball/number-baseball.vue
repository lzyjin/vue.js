<template>
  <div>
    <h1>{{result}}</h1>
    <!-- <form v-on:submit="onSubmitForm"> -->
      <!-- v-on:을 @로 축약할 수 있다 -->
    <!-- <form @submit="onSubmitForm"> -->
    <!-- @submit.prevent 라고 쓰면 event.preventDefault() 를 수행한다 -->
    <form @submit.prevent="onSubmitForm">
      <input ref="answer" minlength="4" maxlength="4" v-model="value">
      <button type="submit">입력</button>
    </form>
    <div>시도: {{tries.length}}</div>
    <ul>
      <!-- <li v-for="try in tries"></li> -->
      <!-- try는 예약어라서 쓸 수 없다 -->
      <!-- vue의 반복문: v-for="값 in 배열", 단 vue 2.2.0이상에서 v-for을 쓰려면 v-bind:key=""가 필수이다. -->
      <!-- <li v-for="t in tries" v-bind:key="t">{{t}}</li> -->
      <li v-for="(t, idx) in tries" v-bind:key="idx">
        <div>{{t.try}}</div>
        <div>{{t.result}}</div>
      </li>
    </ul>
  </div>
</template>

<script>
  // getNumbers라는 함수는 컴포넌트의 methods안에 넣을 수도 있지만, 이 함수는 화면이나 데이터와는 크게 관련이 없기 때문에 분리한 것. 얘는 다른 컴포넌트에서 쓸 수도 있고, 굳이 methods와 엮지 않았다.
  // 만약 methods안에 적었다면 호출할 때 this.getNumbers() 라고 작성해야한다.
  // data나 methods에는 현재 컴포넌트(화면)과 밀접한 관계가 있는 것만 묶어주면 된다.
  // 4자리 랜덤 숫자 만드는 함수 (네 숫자는 겹치지 않는다)
  const getNumbers = () => {
    const candidates = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    const array = [];
    for(let i = 0; i < 4; i++) {
      const chosen = candidates.splice(Math.floor(Math.random() * (9 - i)), 1)[0];
      array.push(chosen);
    }
    return array;
  }

  // export default 의 객체{}가 main.js에서 import NumberBaseball from './number-baseball'; 의 NumberBaseball이 되는것.
  // export default와 import from은 세트다! 
  // 이건 자바스크립트의 모듈 시스템.
  // 노드의 모듈시스템에서는 import대신에 require을 쓰고, export default대신에 module.exports를 쓴다
  export default {
    data() {
      return {
        answer: getNumbers(), // answer은 배열 ex) [1, 4, 5, 6]
        // tries는 내가 어떤 시도를 했는지 값을 담을 배열
        tries: [], // 시도 횟수
        value: '', // 입력값
        result: '' // 결과
      }
    }, 
    methods: {
      onSubmitForm(e) {
        // e.preventDefault();
        // 이 코드도 축약할 수 있다
        // @submit.prevent="" 이렇게

        if(this.value === this.answer.join('')) { //입력값이 정답이면 (value는 input창에 입력한 값이라서 문자열이고, answer은 배열이라서 문자열로 바꿔준 것)

          // this.tries.push(this.value);
          this.tries.push({
            try: this.value,
            result: '홈런'
          });
          this.result = '홈런~!';
          alert('숫자야구 게임을 다시 시작합니다.');
          this.value = '';
          this.answer = getNumbers();
          this.tries = [];
          this.$refs.answer.focus();

        } else { // 정답이 아닐 때

          if(this.tries.length >= 9) { // 10번째 시도

            console.log(`answer: ${this.answer}`);
            this.result = `10번 넘게 틀려서 실패! 답은 ${this.answer.join('')}였습니다.`;
            alert('숫자야구 게임을 다시 시작합니다.');
            this.value = '';
            this.answer = getNumbers();
            this.tries = [];
            this.$refs.answer.focus();

          } else {

            // 어떤것을 변수로 만들고 어떤것을 data로 만드는지 헷갈리다면
            // 화면에 보여지는 것은 data, 계산식같은곳에 쓰이는 화면과는 관련없는 것은 변수로.
            let strike = 0;
            let ball = 0;
            const answerArray = this.value.split('').map( v => parseInt(v) ); // 입력한 네자리 숫자를 잘라서 배열에 담고 그 값들을 정수로 변환해서 숫자배열로 바꾸는것.
            for(let i=0; i < 4; i++) {
              if(answerArray[i] === this.answer[i]) { // 숫자와 자리수 모두 정답
                strike++;
              } else if(this.answer.includes(answerArray[i])) { // 숫자만 정답
                ball++;
              }
            }
            this.tries.push({
              try: this.value,
              result: `${strike} 스트라이크, ${ball} 볼입니다.`
            });
            this.value = '';
            this.$refs.answer.focus();
            
          }

        }

      }
    }
  }
  // 꼭 코드 수정하고 build 다시 해주기 
</script>

<style>

</style>
