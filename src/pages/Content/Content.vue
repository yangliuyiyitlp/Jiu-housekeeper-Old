<template>
  <div >
    <el-row class="sidebar">
      <el-col :span="3" class="sidebar">
        <content-bar></content-bar>
      </el-col>
      <el-col :span="21">
        <router-view></router-view>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import ContentBar from '../../components/commons/Ztree/ContentBar.vue'
  import bus from '../../assets/js/eventBus.js'
  export default {
    data () {
      return {
        bar: ''
      }
    },
    components: {
      ContentBar
    },
    mounted () {
      bus.$on('contentbar', (bar) => {
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
