<template>
  <div id="app">
    <div class="container-row" v-for="(item, index) in items" :id="'b_' + item.desc">
        <h1>{{ item.name }}</h1>
      </span>
    </div>
    <navc :items="items"></navc>
  </div>
</template>

<script>
import navc from './components/bilibili-scroll'

export default {
  name: 'app',
  components: {
    navc
  },
  data() {
    return {
      items: [{
        name: '动画',
        desc: 'douga',
      }, {
        name: '游戏',
        desc: 'game',
      }, {
        name: '音乐',
        desc: 'music',
      }, {
        name: '舞蹈',
        desc: 'dance',
      }, {
        name: '科技',
        desc: 'technology',
      }, {
        name: '生活',
        desc: 'life',
      }, {
        name: '电影',
        desc: 'movie',
      }]
    }
  },
  computed: {
    options() {
      let options = {
        offset: 100,
        data: this.data,
        backToTop: 0
      }
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
  height: 200px;
  font: 30px;
  margin-bottom: 20px;
  background-color: green;
}
h1 {
  height: 200px;
  line-height: 200px;
}
</style>
