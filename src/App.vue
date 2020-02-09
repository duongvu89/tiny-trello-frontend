<template>
  <div id="app">
    <div class="title">TINY<img alt="Vue logo" src="./assets/logo.png">TRELLO</div>
    <board/>
  </div>
</template>

<script>
  import Board from './components/Board.vue'

  export default {
  name: 'app',
  components: {
    Board
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  .title {
    font-size: 38px;
    font-family: cursive;
  }
  .title > img {
    height: 30px;
    margin: 0 5px;
  }
</style>
