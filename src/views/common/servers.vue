<template>
  <div>
    <div v-if="parentShow">
    <div v-for="item in dataList" style="padding:10px" @click="showDetail(item.carrierpsn)">
      <el-card >
          <div class='div_item_title'>
              <span class='span_item_title'>{{item.serverCode}}</span><span style="float:right;margin-top:5px;margin-right:5px;"><el-button size="mini" type="success" round>在线</el-button></span>
          </div>
          <div>
            <el-row :gutter='8'>
              <el-col :span="12">
                <div style="height:30px; margin-top:5px;">
                    <p class="p_content_item" style="font-size:14px;"><span>PSN:</span><span style="margin-left:5px;">{{item.carrierpsn}}</span></p>
                </div>
              </el-col>
              <el-col :span="12">
                <div style="height:30px; margin-top:5px;">
                    <p class="p_content_item" style="font-size:14px;"><span>USN:</span><span style="margin-left:5px; width:190px; overflow: hidden;text-overflow: ellipsis;white-space: nowrap;text-align: right;">{{item.carrierusn}}</span></p>
                </div>
              </el-col>
            </el-row>
            <el-row :gutter='8'>
              <el-col :span="12">
                <div style="height:30px; margin-top:5px;">
                    <p class="p_content_item" style="font-size:14px;"><span>机房名:</span><span style="margin-left:5px;">{{item.roomName}}</span></p>
                </div>
              </el-col>
              <el-col :span="12">
                <div style="height:30px; margin-top:5px;">
                    <p class="p_content_item" style="font-size:14px;"><span>机柜名:</span><span style="margin-left:5px;">{{item.frameName}}</span></p>
                </div>
              </el-col>
            </el-row>
            <el-row :gutter='8'>
              <el-col :span="24">
                <div style="height:auto;margin-top:5px;">
                    <p class="p_content_item" style="font-size:14px;"><span>备注:</span><span style="margin-left:5px;">{{item.comment}}</span></p>
                </div>
              </el-col>
            </el-row>
          </div>
          <div style="margin:10px -20px -10px -20px;padding-top:10px;border-top:1px solid #f2f2f2">
            <el-row >
              <el-col :span="12">
                <div style="height:30px;text-align:center">
                  <el-button size="mini" type="success">查看详情</el-button>
                </div>
              </el-col>
              <el-col :span="12">
                <div style="height:30px;text-align:center">
                  <el-button size="mini" type="success">查看告警</el-button>
                </div>
              </el-col>
            </el-row>
          </div> 
      </el-card>
    </div>

  </div>
  <servers-detail v-if="serversDetailVisible" ref="serversDetailModule" @refreshDataList="showParent"></servers-detail>
  </div>
</template>

<script>
  import serversDetail from './serversDetail'
  export default {
    data () {
      return {
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        topStatus: '',
        parentShow: true,
        serversDetailVisible: false
      }
    },
    created () {
      this.getDataList()
    },
    components: {
      serversDetail
    },
    methods: {
      loadTop () {
        this.$refs.loadmore.onTopLoaded()
      },
      loadBottom () {
        this.$refs.loadmore.onBottomLoaded()
      },
      // 获取数据列表
      getDataList (status) {
        // this.dataListLoading = true
        this.topStatus = status
        this.$http({
          url: this.$http.adornUrl('/server/list'),
          method: 'get',
          params: this.$http.adornParams({
            'pageIndex': this.pageIndex,
            'pageSize': this.pageSize
          }, false)
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.dataList = data.page.list
            this.totalPage = data.page.totalCount
          } else {
            this.dataList = []
            this.totalPage = 0
          }
            // this.dataListLoading = false
        })
      },
      // 显示父组件
      showParent () {
        this.parentShow = true
        this.serversDetailVisible = false
      },
      // 查看服务器详情
      showDetail (psnNum) {
        console.log(psnNum)
        this.parentShow = false
        this.serversDetailVisible = true
        this.$nextTick(() => {
          this.$refs.serversDetailModule.init(psnNum)
        })
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
.div_item_bottom {
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

