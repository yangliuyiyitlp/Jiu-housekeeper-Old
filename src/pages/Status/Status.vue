<template>
  <div>
    <el-row class="sidebar">
      <el-col :span="3" class="sidebar">
        <status-bar></status-bar>
      </el-col>
      <el-col :span="21">
        <router-view></router-view>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import StatusBar from '@/components/commons/Ztree/StatusBar.vue'
  import bus from '@/assets/js/eventBus.js'
  export default {
    data () {
      return {
        bar: ''
      }
    },
    components: {
      StatusBar
    },
    mounted () {
      bus.$on('statusbar', (bar) => {
        this.bar = bar
        console.log(this.bar)
        this.$router.push({
          name: this.bar
        })
      })
    }
  }
</script>

<style scoped>
  html,body,.sidebar{
    height:100%!important;
  }
</style>
