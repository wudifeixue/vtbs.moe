<template>
<el-container>
  <el-main>
    <el-row type="flex" justify="space-around">
      <el-col :xs="24" :sm="20" :md="16" :lg="13" :xl="12" v-loading="!vtbs.length">
        <transition-group name="flip-list">
          <card v-for="vtb in rank" :vtb="vtb" hover :key="vtb.mid" class="card"></card>
        </transition-group>
      </el-col>
    </el-row>
  </el-main>
</el-container>
</template>

<script>
import { mapState, mapGetters } from 'vuex'

import card from '@/components/card'

export default {
  name: 'home',
  components: {
    card,
  },
  computed: { ...mapState(['vtbs']),
    ...mapGetters(['followerRank', 'liveRank', 'riseRank']),
    rank: function() {
      if (this.$route.path.includes('live')) {
        return this.liveRank
      }
      if (this.$route.path.includes('rise')) {
        return this.riseRank
      }
      return this.followerRank
    },
  },
}
</script>

<style scoped>
.flip-list-move {
  transition: transform 0.5s;
}

.card {
  margin-bottom: 32px;
}
</style>
