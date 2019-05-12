<template>
  <div class="container">
    <div class="stack-wrapper">
      <stack></stack>
    </div>
  </div>
</template>

<script>
  import stack from './components/Stack'
  export default {
    data () {
      return {
      }
    },
    components: {
      stack
    },
    methods: {
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