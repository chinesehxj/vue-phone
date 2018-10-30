<template>
<div class="page-tabbar">
  <!-- <mt-header :title="titleName" >
  </mt-header> -->
  <div class="page-wrap">
    <mt-tab-container v-model="selected">
      <mt-tab-container-item id="tab1">
        <outline-data></outline-data>
      </mt-tab-container-item>
      <mt-tab-container-item id="tab2">
        <server-list></server-list>
      </mt-tab-container-item>
      <mt-tab-container-item id="tab3">
        <storage></storage>
      </mt-tab-container-item>
      <mt-tab-container-item id="tab4">
        <my-center></my-center>
      </mt-tab-container-item>
    </mt-tab-container>
  </div>
  <mt-tabbar v-model="selected">
    
    <mt-tab-item id="tab1">
      <i class="iconfont el-icon-dms-shuju" style="font-size:30px;"></i></br>
      数据总览
    </mt-tab-item>
    <mt-tab-item id="tab2">
      <i class="iconfont el-icon-dms-fuwuqi1" style="font-size:30px;"></i></br>
      服务器
    </mt-tab-item>
    <mt-tab-item id="tab3">
      <i class="iconfont el-icon-dms-yingpan" style="font-size:30px;"></i></br>
      存储
    </mt-tab-item>
    <mt-tab-item id="tab4">
      <i class="iconfont el-icon-dms-user" style="font-size:30px;"></i></br>
      个人中心
    </mt-tab-item>
  </mt-tabbar>
</div>
</template>

<script>
  import outlineData from './outline'
  import storage from './storage'
  import serverList from './servers'
  import myCenter from './my'
  export default {
    name: 'page-tabbar',
    data () {
      return {
        selected: 'tab1',
        titleName: '数据总览'
      }
    },
    components: {
      outlineData,
      storage,
      serverList,
      myCenter
    }
    // watch: {
    //   selected: function () {
    //     if (this.selected === 'tab1') {
    //       this.titleName = '数据总览'
    //     } else if (this.selected === 'tab2') {
    //       this.titleName = '服务器一览'
    //     } else if (this.selected === 'tab3') {
    //       this.titleName = '告警一览'
    //     } else {
    //       this.titleName = '个人中心'
    //     }
    //   }
    // }
  }
</script>

<style>
  .page-tabbar {
    overflow: hidden;
    height: 100vh;
  }
 
  .page-wrap {
    overflow: auto;
    height: 100%;
    padding-bottom: 60px;
  }
</style>

