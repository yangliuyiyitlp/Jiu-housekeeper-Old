<template>
  <div class="sidebar">
    <el-tree
      class="sidebar"
      :data="data"
      :props="defaultProps"
      default-expand-all
      highlight-current
      @node-click="handleNodeClick">
    </el-tree>
  </div>
</template>
<script>
  import bus from '@/assets/js/eventBus.js'

  export default {
    data () {
      return {
//        data: [{
//          label: '操作明细',
//          children: [{
//            label: '会员审核明细',
//            name: 'report.vip'
//          }, {
//            label: '手工还车明细',
//            name: 'report.manual.return'
//          }]
//        }, {
//          label: '借还车明细',
//          children: [{
//            label: '交易记录',
//            name: 'report.transaction'
//          }, {
//            label: '当日交易记录',
//            name: 'report.transaction.now'
//          }, {
//            label: '换电池明细',
//            name: 'report.replace.battery'
//          }, {
//            label: '充值提现明细',
//            name: 'report.recharge'
//          }]
//        }],
        data: [],
        defaultProps: {
          children: 'children',
          label:
            'label'
        }
      }
    },
    mounted () {
      let menu = sessionStorage.getItem('menus')
      this.menus = JSON.parse(menu)
      for (let key in this.menus) {
        if (key === 'report') {
          this.data = this.menus[key]
        }
      }
    },
    methods: {
      handleNodeClick (data) {
        console.log(data)
        bus.$emit('reportbar', data.name)
      }
    }
  }
</script>


<style scoped>
  html, body, .sidebar {
    height: 100% !important;
  }
</style>

