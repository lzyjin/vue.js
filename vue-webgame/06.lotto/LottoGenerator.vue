<template>
  <div>
    <!-- v-bind:class와 v-bind:style은 조금 독특하다. 값에 객체형식을 넣을 수 있다 -->
    <!-- state가 true라서 state만 class가 될 수 있다.  -->
    <!-- 자바스크립트에서 css속성을 사용할 때는 -을 빼기기호로 인식하기때문에 대문자로 바꿔서 카멜케이스로 만들어야한다 -->
    <!-- <div id="computer" :class="{ state: true, hello: false }" v-bind:style="{ backgroundImage: '', fontSize: '14px' }"></div> -->

    <!-- imgCoord라는 데이터만 사용하는 것이 아니라 문자열과 붙어있으므로 이런것을 computed로 만든다 => 캐싱효과 -->
    <!-- <div id="computer" :style="{ background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${imgCoord} 0`}"></div> -->
    <div id="computer" :style="computedStyleObject"></div>
    <div>
      <button @click="onClickButton('바위')">바위</button>
      <button @click="onClickButton('가위')">가위</button>
      <button @click="onClickButton('보')">보</button>
    </div>
    <div>{{result}}</div>
    <div>현재 {{score}}점</div>
    <!-- <lifecycle-example v-if="true"></lifecycle-example> -->
  </div>
</template>

<script>
  const rspCoords = {
    바위: '0',
    가위: '-142px',
    보: '-284px'
  };
  const scores = {
    가위: 1,
    바위: 0,
    보: -1
  };
  const computerChoice =(imgCoord) => {
    return Object.entries(rspCoords).find( function(v) {
      return v[1] === imgCoord;
    } )[0];
  };
  let interval = null;
  export default {
    data() {
      return {
        imgCoord: rspCoords.바위,
        result: '',
        score: 0
      }
    },
    computed: {
      computedStyleObject(){
          return{
            background:`url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0`
          }
        }
    },
    methods: {
      changeHand() {
        interval = setInterval( () => {
          if( this.imgCoord === rspCoords.바위 ) {
            this.imgCoord = rspCoords.가위;
          } else if( this.imgCoord === rspCoords.가위 ) {
            this.imgCoord = rspCoords.보;
          } else if( this.imgCoord === rspCoords.보 ) {
            this.imgCoord = rspCoords.바위;
          }
        }, 100 );
      },
      onClickButton(choice) {
        clearInterval(interval);
        const myScore = scores[choice];
        const cpuScore = scores[computerChoice(this.imgCoord)];
        const diff = myScore - cpuScore;
        if(diff === 0) {
          this.result = '비겼습니다';
        } else if( [-1, 2].includes(diff) ) {
          this.result = '이겼습니다';
          this.score += '1';
        } else {
          this.result = '졌습니다';
          this.score -= 1;
        }
        setTimeout( () => {
          this.changeHand();
        }, 1000 );
      }
    },
    beforeCreate() {
      console.log('beforeCreate');
    },
    created() {
      // created는 data가 다 준비된 상태로 컴포넌트가 보여지게될 때 (화면에 나타나기 전)
      // 자바스크립트 상으로만 존재함
      console.log('created');
    },
    beforeMount() {
      console.log('beforeMount');
    },
    mounted() {
      // mounted는 컴포넌트가 화면에 나타난 후
      // 화면에 존재함
      console.log('mounted');
      // setInterval을 변수에 담아야 나중에 변수로 접근해서 clearInterval할 수 있다 (반복을 멈출 수 있다)
      this.changeHand();
    },
    beforeUpdate() {
      console.log('beforeUpdate');
    },
    updated() {
      // updated는 화면의 데이터가 바뀌어서 화면이 다시 그려질 때
      console.log('updated');
    },
    beforeDestroy() {
      console.log('beforeDestroy');
      // 중요 
      // setTimeout이나 setInterval을 끝내야 메모리 누수 문제가 일어나지 않는다
      clearInterval(interval);
    },
    destroyed() {
      // destroyed는 컴포넌트가 화면에서 사라질 때 
      console.log('destroyed');
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
