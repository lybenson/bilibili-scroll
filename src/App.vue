<template>
  <div id="app">
    <div style="background-color: red; width: 100%; height: 500px;"></div>
    <div class="container-row" v-for="(item, index) in items" :id="item.id">
        <h1>{{ item.name }}</h1>
      </span>
    </div>
    <NavSide :options="options" style="z-index: 10000;"></NavSide>
  </div>
</template>

<script>
import NavSide from './components/NavSide'

export default {
  name: 'app',
  components: {
    NavSide
  },
  data() {
    return {
      items: [{
        name: '动画',
        id: 'b_douga'
      }, {
        name: '游戏',
        id: 'b_game'
      }, {
        name: '音乐',
        id: 'b_music'
      }, {
        name: '舞蹈',
        id: 'b_dance'
      }, {
        name: '科技',
        id: 'b_technology'
      }, {
        name: '生活',
        id: 'b_life'
      }, {
        name: '电影',
        id: 'b_movie'
      }]
    }
  },
  computed: {
    options() {
      let options = {
        offset: 100, //偏移的距离
        items: this.items,
        offsetTop: 0 //距离顶部距离
      }
      return options
    }
  }
}
</script>

<style>
#app {
  font-family: "Segoe UI", 'Microsoft Yahei', Helvetica, Arial;
  text-align: center;
  color: #2c3e50;
}

.container-row {
  width: 100%;
  height: 400px;
  font: 30px;
  margin-bottom: 20px;
  background-color: green;
}
h1 {
  height: 200px;
  line-height: 200px;
}
</style>
