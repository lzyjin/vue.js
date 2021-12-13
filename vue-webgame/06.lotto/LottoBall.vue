<template>
  <div class="ball" :style="{ background }">{{ number }}</div>
</template>

<script>

// 현재 LottoGenerator 컴포넌트가 부모컴포넌트, LottoBall컴포넌트가 자식컴포넌트
// 부모컴포넌트에서 자식컴포넌트의 태그부분에 속성처럼 number="5"라고 적으면 
// 자식컴포넌트에서 props속성으로 number을 받아올 수 있다
// props: 자식컴포넌트에게 값을 물려주는 것
export default {
  name: 'LottoBall',
  // props: {
  //   number: Number
  // },
  // 축약형
  props: ['number'],
  data() {
    let background;
    return {
      background,
      // numberData: this.number;
      // props로 받아온 값을 바꾸고 싶어서 이렇게 하면 
      // 가능은 하지만 이러면 부모의 값은 그대로고 numberData는 바뀌기 때문에 데이터가 불일치하게된다. -> 데이터 관리가 어려워진다 -> 비추
      // 그래서 부모의 값 자체를 바꾸는 것이 좋다
    }
  },
  methods: {
    // 이렇게 number값을 바꾸는것은 불가능하다
    // props로 받아온 값은 readonly라고 생각하자
    // changeNumber() {
    //   this.number =5;
    // }
  },
  computed: {

  },
  mounted() {
    
  },
  beforeDestory() {

  },
  watch: {

  }
}
// node_modules가 없으면 실행할 수 없다
// npm i 로 노드모듈들을 불러오고, npm run dev로 실행해야한다
</script>

<style scoped>
.ball {
  display: inline-block;
  border: 1px solid #000;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  line-height: 40px;
  font-size: 20px;
  text-align: center;
  margin-right: 20px;
}
</style>