<template>
  <div>
    <el-row class="sidebar">
      <el-col :span="3" class="sidebar">
        <service-bar></service-bar>
      </el-col>
      <el-col :span="21">
        <router-view></router-view>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import ServiceBar from '@/components/commons/Ztree/ServiceBar.vue'
  import bus from '@/assets/js/eventBus.js'
  export default {
    data () {
      return {
        bar: ''
      }
    },
    components: {
      ServiceBar
    },
    mounted () {
      bus.$on('servicebar', (bar) => {
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
