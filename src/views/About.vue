<template>
<el-container>
  <el-main>
    <el-row>
      <el-col :xs="24" :span="12">
        <h1>关于</h1>
        <p>🍉 按照关注数排列<br>
          直播势：直播中的按照人气排列，靠前，其他按照舰队排列<br>
          宏观经济：bilibili 虚拟世界宏观走势<br>
          数据每 5 分钟更新一次<br>
          宏观经济中视频势每 6 小时更新一次<br>
          风云榜，24小时更新一次 <br>
          名单查漏补缺 issue: <a href="https://github.com/simon300000/vtbs.moe/issues/1">https://github.com/simon300000/vtbs.moe/issues/1</a>
          <br>
          日增的数据是过去24小时粉丝数变化，并不是昨天一天的变化
        </p>
        <a href="https://github.com/simon300000/vtbs.moe/" target="_blank"><img alt="GitHub stars" src="https://img.shields.io/github/stars/simon300000/vtbs.moe.svg?style=social"></a> <br>
        <a href="https://github.com/simon300000/vtbs.moe/" target="_blank">github:simon300000/vtbs.moe</a>
      </el-col>
      <el-col :xs="24" :span="12">
        <h1>服务器数据：</h1>
        <p v-loading="!spiders">Spiders: {{spiders}}</p>
        <p v-loading="!interval">Interval: {{interval}} ms</p>
        <p v-loading="!number">共收录VTB/VUP: {{number}} 个</p>
        <p v-if="online">目前在线: {{online}}</p>
        <div v-for="{time, spiderId, duration} in spiderUpdate" :key="`spider_${spiderId}`">
          <h4>Spiders {{spiderId}}</h4>
          <p>上次更新: {{time | parseTime}} <br>
            目前负载: {{duration | load(interval)}}</p>
        </div>
        <h1>logs:</h1>
        <el-timeline>
          <el-timeline-item v-for="(log, index) in [...logs].reverse()" :key="index" :timestamp="log.time">
            {{log.data}}
          </el-timeline-item>
        </el-timeline>
      </el-col>
    </el-row>
  </el-main>
</el-container>
</template>

<script>
import { mapState } from 'vuex'
import moment from 'moment'

export default {
  computed: { ...mapState(['logs', 'status', 'spiderUpdate', 'online', 'vtbs']),
    spiders: function() {
      return this.status.PARALLEL
    },
    interval: function() {
      return this.status.INTERVAL
    },
    number: function() {
      return this.vtbs && this.vtbs.length
    },
  },
  filters: {
    parseTime: function(time = 0) {
      let timeNow = moment(time)
      return `${timeNow.hours()}:${timeNow.minute()}`
    },
    load: function(duration, interval) {
      return `${Math.round(duration / interval * 1000) / 10}%`
    },
  },
}
</script>

<style scoped>
.el-main {
  word-wrap: break-word;
}
</style>
