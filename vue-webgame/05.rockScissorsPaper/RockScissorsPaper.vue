<template>
  <div>
    <!-- v-bind:class와 v-bind:style은 조금 독특하다. 값에 객체형식을 넣을 수 있다 -->
    <!-- state가 true라서 state만 class가 될 수 있다.  -->
    <!-- 자바스크립트에서 css속성을 사용할 때는 -을 빼기기호로 인식하기때문에 대문자로 바꿔서 카멜케이스로 만들어야한다 -->
    <!-- <div id="computer" :class="{ state: true, hello: false }" v-bind:style="{ backgroundImage: '', fontSize: '14px' }"></div> -->

    <!-- imgCoord라는 데이터만 사용하는 것이 아니라 문자열과 붙어있으므로 이런것을 computed로 만든다 => 캐싱효과 -->
    <!-- <div id="computer" :style="{ background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${imgCoord} 0`}"></div> -->
    <div id="computer" :style="{ computedStyleObject }"></div>
    <div>
      <button @click="onClickButton('바위')">바위</button>
      <button @click="onClickButton('가위')">가위</button>
      <button @click="onClickButton('보')">보</button>
    </div>
    <div>{{result}}</div>
    <div>현재 {{score}}점</div>
  </div>
</template>

<script>
  const rspCoords = {
    바위: '0',
    가위: '-142px',
    보: '-284px'
  }
  export default {
    data() {
      return {
        imgCoord: rspCoords.바위,
        result: '',
        score: 0
      }
    },
    computed: {
      computedStyleObject() {
        return {
          background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0`
        }
      }
    },
    methods: {
      onClickButton(choice) {

      }
    }
  }
</script>

<style scoped>
  #computer {
    width: 142px;
    height: 200px;
    background-position: 0 0;
  }
</style>
