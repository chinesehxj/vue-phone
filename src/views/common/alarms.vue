<template>
  <div>
    
    <div style="padding:10px">
      <el-card>
          <div class='div_item_title'>
              <span class='span_item_title'>计算机名(编号)</span>
          </div>
          <div>
            <el-row>
              <el-col :span="24">
                <div style="height:30px; margin-top:5px;">
                    <p class="p_content_item" style="font-size:14px;"><span>告警内容:</span><span style="margin-left:5px;">磁盘空间不足</span></p>
                </div>
              </el-col>
            </el-row>
            <el-row :gutter='8'>
              <el-col :span="12">
                <div style="height:30px; margin-top:5px;">
                    <p class="p_content_item" style="font-size:14px;"><span>机房名:</span><span style="margin-left:5px;">3号机房</span></p>
                </div>
              </el-col>
              <el-col :span="12">
                <div style="height:30px; margin-top:5px;">
                    <p class="p_content_item" style="font-size:14px;"><span>机柜名:</span><span style="margin-left:5px;">小号机柜</span></p>
                </div>
              </el-col>
            </el-row>
            <el-row :gutter='8'>
              <el-col :span="24">
                <div style="height:auto;margin-top:5px;">
                    <p class="p_content_item" style="font-size:14px;"><span>告警时间:</span><span style="margin-left:5px;">2018-7-31 19:25:36</span></p>
                </div>
              </el-col>
            </el-row>
          </div> 
      </el-card>
    </div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
      }
    }
  }
</script>

<style>
.div_title {
  margin-top: -20px;
  margin-left: -20px;
  margin-right: -20px;
  border-bottom: 1px solid #eaeef2;
  background-color: white;
}
.span_serverName {
  margin-left: 20px;
  line-height: 50px;
  font-size: 18px;
  font-weight: bold;
  color: dimgrey;
}
.content {
  margin-top: 20px;
}
.div_item_title {
  margin-top: -20px;
  margin-left: -20px;
  margin-right: -20px;
  height: 40px;
  text-align: left;
  border-bottom: 1px solid #eaeef2;
}
.span_item_title {
  line-height: 40px;
  font-size: 16px;
  margin-left: 10px;
}
.div_temp {
  overflow: hidden;
  margin-top: 8px;
}
.div_temp_name {
  width:100px;
  float:left;
}
.div_temp_progress {
  width:400px;
  float:left;
}
.temp_card {
    background-color: #b3d8ff;
}
.vol_card {
    background-color: #c2e7b0;
}
.p_content_item {
    line-height: 30px;height:30px;font-size:14px;
}
.userate_card {
    background-color: #f5dab1;
}
</style>

