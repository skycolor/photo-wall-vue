<template>
  <div id="app">
    <MainPhotoWall></MainPhotoWall>
  </div>
</template>

<script>
import MainPhotoWall from './components/MainPhotoWall'

export default {
  name: 'app',
  components: {
    MainPhotoWall
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
  margin-top: 60px;
}
</style>
