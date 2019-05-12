<template>
  <div class="container">
    <div class="stack-wrapper">
      <stack ref="stack" :pages="someList" :stackinit="stackinit"></stack>
    </div>
  </div>
</template>

<script>
  import stack from './components/Stack'
  export default {
    data () {
      return {
        someList: [],
        stackinit: {
          visible: 3
        }
      }
    },
    mounted () {
      let that = this
      setTimeout(function () {
        that.someList = [
          {
            html: '<img src="https://img.xiangshengclub.com/MTU1NjM3MjY0NTYxNCMxMDAjanBn.jpg" alt="01">'
          },
          {
            html: '<img src="https://img.xiangshengclub.com/MTU1NjM3MjY0NTYxNCMxMDAjanBn.jpg" alt="02">'
          },
          {
            html: '<img src="https://img.xiangshengclub.com/MTU1NjM3MjY0NTYxNCMxMDAjanBn.jpg" alt="03">'
          },
          {
            html: '<img src="https://img.xiangshengclub.com/MTU1NjM3MjY0NTYxNCMxMDAjanBn.jpg" alt="04">'
          },
          {
            html: '<img src="https://img.xiangshengclub.com/MTU1NjM3MjY0NTYxNCMxMDAjanBn.jpg" alt="05">'
          },
          {
            html: '<img src="https://img.xiangshengclub.com/MTU1NjM3MjY0NTYxNCMxMDAjanBn.jpg" alt="06">'
          },
          {
            html: '<img src="https://img.xiangshengclub.com/MTU1NjM3MjY0NTYxNCMxMDAjanBn.jpg" alt="07">'
          }
        ]
      }, 2000)
    },
    components: {
      stack
    },
    methods: {
      prev () {
        this.$refs.stack.$emit('prev')
      },
      next () {
        this.$refs.stack.$emit('next')
      }
    }
  }
</script>

<style>
  body {
    margin: 0;
    padding: 0;
    min-width: 100vw;
    min-height: 100vh;
    background: url('./assets/bg.jpg') no-repeat;
    background-size: cover;
  }

  .stack-wrapper {
    margin: 0 auto;
    padding-top: 10vh;
    position: relative;
    z-index: 1000;
    width: 80vw;
    height: 50vh;
    list-style: none;
    pointer-events: none;
  }
</style>