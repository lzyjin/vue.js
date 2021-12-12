<template>
  <div>
    <div>당첨 숫자</div>
    <lotto-ball v-for="ball in winBalls" v-bind:key="ball.idx"></lotto-ball>
    <div>보너스</div>
    <lotto-ball v-if="bonus"></lotto-ball>
    <button v-if="redo">한번 더!</button>
  </div>
</template>

<script>
export default {
    data() {
      return {
        winBalls: [],
        bonus: null,
        redo: fales,
      }
    },
    computed: {
    },
    methods: {
    },
    mounted() {
    },
    beforeDestroy() {
    },
    watch: {

    }
  }
</script>

<style scoped>

</style>