<template>
<div>
  <mt-header title="多个按钮">
      <div slot="left">
      <mt-button v-link="'/'" icon="back">返回</mt-button>
      <mt-button @click="handleClose">关闭</mt-button>
    </div>
    <mt-button icon="more" slot="right"></mt-button>
  </mt-header>
</div>

<div>
  <mt-tabbar v-model="selected">
    
    <mt-tab-item id="tab1">
      <i class="iconfont el-icon-dms-portal-icon-bangdingxunijiwangqia" style="font-size:30px;;margin-top:5px"></i>
      tab1
    </mt-tab-item>
    <mt-tab-item id="tab2">
      <i class="iconfont el-icon-dms-portal-icon-bangdingxunijiwangqia" style="font-size:30px;;margin-top:5px"></i>
      tab2
    </mt-tab-item>
    <mt-tab-item id="tab3">
      <i class="iconfont el-icon-dms-portal-icon-bangdingxunijiwangqia" style="font-size:30px;;margin-top:5px"></i>
      tab3
    </mt-tab-item>
    <mt-tab-item id="tab4">
      <i class="iconfont el-icon-dms-portal-icon-bangdingxunijiwangqia" style="font-size:30px;;margin-top:5px"></i>
      tab4
    </mt-tab-item>
  </mt-tabbar>
</div>
</template>

<script>
  export default {
  }
</script>

<style>
 
</style>

