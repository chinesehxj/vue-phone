<style>
.detail_serverName {
  margin-left: 5px;
  line-height: 30px;
  font-size: 14px;
  font-weight: bold;
  color: dimgrey;
}

.detail_item_title {
  margin-top: -20px;
  margin-left: -20px;
  margin-right: -20px;
  height: 30px;
  text-align: center;
  border-bottom: 1px solid #eaeef2;
}
.span_item_title {
  line-height: 30px;
  font-size: 14px;
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
    background-color: #ffffff;
}
.vol_card {
    background-color: #ffffff;
}
.p_content_item {
    line-height: 30px;height:30px;font-size:14px;
}
.userate_card {
    background-color: #ffffff;
}
</style>
<template>
        <div style="background-color:#f2f2f2;">
                <mt-header title="服务器详情">
      
                </mt-header>
                <div style="padding-bottom:3px;border-bottom: 1px solid #eaeef2;background-color:#ffffff;position:fixed;width:100%;top:40px;z-index:99">
                    <el-row>
                        <el-col :span='18'>
                            <span class='detail_serverName'>{{computerName}} 详细信息</span>
                            <span style="font-size: 12px;margin-left: 5px;color: #919191;display:block;">(最新数据采集于:{{collectTime}})</span>
                        </el-col>
                        <el-col :span='6'>
                            <span class='detail_serverName' style="float:right;margin-right:10px;margin-top:10px;">
                                <el-button @click="goBack" size="small" icon="el-icon-back" type="primary" plain>返回</el-button>
                            </span>
                        </el-col>
                    </el-row>
                </div>  
            <div style="margin-top:50px">
                <el-row style=" padding-left:5px; padding-right:5px;">
                    <el-col :span="24" style="margin-top:10px;">
                        <el-card shadow='hover'>
                            <div class='detail_item_title userate_card'>
                                <span class='span_item_title '>CPU使用率</span>
                            </div>
                            <div id="lineChart" style="width:100%;height:200px;" ></div>
                        </el-card>
                    </el-col>
                </el-row>
                <el-row style=" padding-left:5px; padding-right:5px;">
                    <el-col :span="24" style="margin-top:10px;">
                        <el-card shadow='hover'>
                            <div class='detail_item_title temp_card'>
                                <span class='span_item_title '>CPU温度</span>
                            </div>
                            <div>
                                <div v-if="cpuInfo.Temperature == null || cpuInfo.Temperature == ''">
                                    <p  style="height:20px;line-height:20px;font-size:12px;"><span>暂无数据</span></p>
                                </div>
                                <div v-else>
                                    <el-row>
                                        <div v-for="item in cpuInfo.Temperature">
                                            <div v-show="isNewItem(item.deviceName)" style="height:30px; margin-top:10px;">
                                                <p class="p_content_item" style="font-size:12px;"><span>{{item.deviceName}}</span><span style="float:right;">{{item.sensorValue}}℃</span></p>
                                            </div>
                                        </div>
                                    </el-row>
                                </div>
                            </div>
                        </el-card>
                    </el-col>
                </el-row>
                <el-row style=" padding-left:5px; padding-right:5px;">
                    <el-col :span="24" style="margin-top:10px;">
                        <el-card shadow='hover'>
                            <div class='detail_item_title vol_card'>
                                <span class='span_item_title '>CPU电压</span>
                            </div>
                            <div v-if="cpuInfo.Voltage == null || cpuInfo.Voltage == ''">
                                <p  style="height:20px;line-height:20px;font-size:12px;"><span>暂无数据</span></p>
                            </div>
                            <div v-else>
                                <div v-for="VoltageItem in cpuInfo.Voltage">
                                    <div v-show="isNewItem(VoltageItem.deviceName)" style="height:30px; margin-top:10px;  border-bottom:1px solid #eaeef2">
                                        <p class="p_content_item">{{VoltageItem.deviceName}}</p>
                                    </div>
                                    <div>
                                        <p class="p_content_item" style="font-size:12px;"><span>{{VoltageItem.sensorName}}</span><span style="float:right;">{{VoltageItem.sensorValue}} V</span></p>
                                    </div>
                                </div>
                            </div>
                    </el-card>
                    </el-col>
                </el-row>
                           
                <el-row style="margin-top:10px; padding-left:5px; padding-right:5px;">
                    <el-col :span='24'>
                        <el-card shadow='hover'>
                            <div class='detail_item_title'>
                                <span class='span_item_title'>主板信息</span>
                            </div>
                            <div v-if="mainBoardList == null || mainBoardList == ''">
                                <p  style="height:20px;line-height:20px;font-size:12px;"><span>暂无数据</span></p>
                            </div>
                            <div v-else>
                                <div style="height:30px; margin-top:10px;  border-bottom:1px solid #eaeef2">
                                    <p class="p_content_item" style="font-size:14px;"><span>计算机名:</span><span style="float:right;">{{computerName}}</span></p>
                                </div>
                                <div style="height:30px; margin-top:10px;  border-bottom:1px solid #eaeef2">
                                    <p class="p_content_item" style="font-size:14px;"><span>主板型号:</span><span style="float:right; width:190px; overflow: hidden;text-overflow: ellipsis;white-space: nowrap;text-align: right;">{{mainBoardVender}}</span></p>
                                </div>
                                <div style="height:auto;margin-top:10px;  border-bottom:1px solid #eaeef2" v-for="item in mainBoardList">
                                    <p class="p_content_item" style="font-size:14px;"><span>{{item.sensorName}}</span><span style="float:right;">{{item.sensorValue}}{{item.sensorUnit}}</span></p>
                                    <p style="height:20px" v-show="item.sensorClass == 'Utilization'">
                                        <el-progress :stroke-width="8" :percentage="parseFloat(item.sensorValue)" :show-text="false" color="#67C23A"></el-progress>
                                    </p>
                                </div>
                            </div> 
                        </el-card>
                        <el-card shadow='hover' style="margin-top:10px">
                            <div class='detail_item_title'>
                                <span class='span_item_title'>内存信息</span>
                            </div>
                            <div v-if="memoryList == null || memoryList == ''">
                                <p  style="height:20px;line-height:20px;font-size:12px;"><span>暂无数据</span></p>
                            </div>
                            <div v-else>
                                <div v-for="item in memoryList">
                                    <div style="height:30px; margin-top:10px;  border-bottom:1px solid #eaeef2">
                                        <p class="p_content_item">{{item.deviceName}}</p>
                                    </div>
                                    <div>
                                        <p class="p_content_item"><span>{{item.sensorName}}</span><span style="float:right;">{{item.sensorValue}} {{item.sensorUnit}}</span></p>
                                    </div>
                                </div> 
                            </div>
                            
                        </el-card>
                    </el-col>
                </el-row>
                <el-row style="margin-top:10px;padding-left:5px; padding-right:5px;">
                    <el-col :span='24'>
                        <el-card shadow='hover'>
                            <div class='detail_item_title'>
                                <span class='span_item_title'>网卡信息</span>
                            </div>
                            <div v-if="networkIntface == null || networkIntface == ''">
                                <p  style="height:20px;line-height:20px;font-size:12px;"><span>暂无数据</span></p>
                            </div>
                            <div v-else>
                                <div v-for="(item,index) in networkIntface">
                                    <div v-if="(index % 2) == 0">
                                    <div style="height:auto;margin-top:10px;  border-bottom:1px solid #eaeef2" >
                                        <p class="p_content_item" style="font-size:14px; height:auto;" >{{item.deviceName}}</p>
                                    </div>
                                    <div>
                                        <p class="p_content_item" style="font-size:12px; height:auto;"><span>{{item.sensorName}}</span><span style="float:right;">{{item.sensorValue}} {{item.sensorUnit}}</span></p>
                                    </div>
                                    </div>
                                    <div style="height:auto; border-bottom:1px solid #eaeef2" v-else>
                                        <p class="p_content_item" style="font-size:12px; height:auto;"><span>{{item.sensorName}}</span><span style="float:right;">{{item.sensorValue}} {{item.sensorUnit}}</span></p>
                                    </div>
                                </div> 
                            </div>
                        </el-card>
                    </el-col>
                </el-row>
                <el-row style="margin-top:10px;padding-left:5px; padding-right:5px;">
                    <el-col :span='24'>
                        <el-card shadow='hover'>
                            <div class='detail_item_title'>
                                <span class='span_item_title'>硬盘信息</span>
                            </div>
                            <div v-if="driverList == null || driverList == ''">
                                <p  style="height:20px;line-height:20px;font-size:12px;"><span>暂无数据</span></p>
                            </div>
                            <div v-else>
                                <div v-for="item in driverList">
                                    <div style="height:30px; margin-top:10px; border-bottom:1px solid #eaeef2" v-show="isNewItem(item.deviceName)">
                                        <p class="p_content_item" style="font-size:14px;">{{item.deviceName}}</p>
                                    </div>
                                    <div>
                                        <p class="p_content_item" style="font-size:12px;"><span>{{item.sensorName}}</span><span style="float:right;">{{item.sensorValue}} {{item.sensorUnit}}</span></p>
                                        <p style="height:20px" v-show="item.sensorClass == 'Utilization'">
                                            <el-progress :stroke-width='8' :percentage="parseFloat(item.sensorValue)" :show-text='false' color="#67C23A"></el-progress>
                                        </p>
                                    </div>
                                </div> 
                            </div>
                        </el-card>
                    </el-col>
                </el-row>
                <el-row style="margin-top:10px;padding-left:5px; padding-right:5px;">
                    <el-col :span='24'>
                        <el-card shadow='hover'>
                            <div class='detail_item_title'>
                                <span class='span_item_title'>显卡信息</span>
                            </div>
                            <div v-if="displayAdapterList == null || displayAdapterList == ''">
                                <p  style="height:20px;line-height:20px;font-size:12px;"><span>暂无数据</span></p>
                            </div>
                            <div v-else>
                                <div v-for="item in displayAdapterList">
                                    <div style="height:30px; margin-top:10px; border-bottom:1px solid #eaeef2" v-show="isNewItem(item.deviceName)">
                                        <p class="p_content_item" style="font-size:14px;">{{item.deviceName}}</p>
                                    </div>
                                    <div>
                                        <p class="p_content_item" style="font-size:12px;"><span>{{item.sensorName}}</span><span style="float:right;">{{item.sensorValue}} {{item.sensorUnit}}</span></p>
                                        <p style="height:20px" v-show="item.sensorClass == 'Utilization'">
                                            <el-progress :stroke-width='8' :percentage="parseFloat(item.sensorValue)" :show-text='false' color="#67C23A"></el-progress>
                                        </p>
                                    </div>
                                </div> 
                            </div>
                        </el-card>
                    </el-col>
                </el-row>
            </div>
        </div>
</template>

<script>
import echarts from 'echarts'
require('echarts/theme/macarons')
let newItemNameFlag = ''
var xAxisData = []
var legendData = []
var seriesData = []
var IntervalObject = ''
export default {
  data () {
    return {
      dataForm: {
        timeRange: '',
        cautionId: '',
        cautionText: ''
      },
      options: [],
      activeName: '1',
      collectTime: '',
      networkIntfaceName: '',
      pageIndex: 1,
      pageSize: 10,
      totalPage: 0,
      mainBoardList: [],
      networkIntface: [],
      memoryList: [],
      driverList: [],
      cpuInfo: '',
      viewerList: [],
      computerName: '',
      mainBoardVender: '',
      cautionDataList: [],
      dataListLoading: false,
      dataListSelections: [],
      addOrUpdateVisible: false,
      paramsPsn: '',
      displayAdapterList: '',
      detailShowFlag: false,
      cautionMainBoard: '',
      cautionCpu: '',
      cautionMemory: '',
      cautionNetwork: '',
      cautionDriver: '',
      cautionDisplay: ''
    }
  },
  watch: {
    activeName: function (val) {
      if (val === '2') {
        this.getCautionList()
      }
    }
  },
  methods: {
    goBack () {
      clearInterval(IntervalObject)
      this.$emit('refreshDataList')
    },
    isNewItem (itemName) {
      if (newItemNameFlag === itemName) {
        return false
      } else {
        newItemNameFlag = itemName
        return true
      }
    },
    init (psn) {
      // 初始化数据
      xAxisData.splice(0, xAxisData.length)
      legendData.splice(0, legendData.length)
      seriesData.splice(0, seriesData.length)
      this.paramsPsn = psn
      this.getDataList()
      var vmObject = this
      IntervalObject = setInterval(
        function () {
          vmObject.getDataList()
        }, 120000)
    },
    getDataList () {
      this.$http({
        url: this.$http.adornUrl('/device/info'),
        method: 'get',
        params: this.$http.adornParams({
          'carrierpsn': this.paramsPsn
        }, false)
      }).then(({data}) => {
        if (data && data.code === 0) {
          this.mainBoardList = data.info.Mainboard
          // this.serverName = data.info.Mainboard[0].computerName
          this.collectTime = data.info.Mainboard[0].dateTimeStr
          this.computerName = data.info.Mainboard[0].computerName
          this.mainBoardVender = data.info.Mainboard[0].deviceName
          this.networkIntface = data.info.NetworkInterface
          this.memoryList = data.info.MemoryModule
          this.driverList = data.info.Drive
          this.displayAdapterList = data.info.DisplayAdapter
        } else {}
      })
      this.$http({
        url: this.$http.adornUrl('/device/cpuInfo'),
        method: 'get',
        params: this.$http.adornParams({
          'carrierpsn': this.paramsPsn
        }, false)
      }).then(({data}) => {
        if (data && data.code === 0) {
          console.log(data)
          this.cpuInfo = data.info
          charData(data.info.Utilization)
        } else {}
      })
    },
    getCautionList () {
      this.dataListLoading = true
      this.$http({
        url: this.$http.adornUrl('/sys/sysparam/selectCautionItems'),
        method: 'get',
        params: this.$http.adornParams({
          'param_type': 'caution_type',
          'parent_id': 0
        }, false)
      }).then(({data}) => {
        this.options = data && data.code === 0 ? data.page : []
      })
      this.$http({
        url: this.$http.adornUrl('/device/noticeInfo'),
        method: 'post',
        data: this.$http.adornData({
          'carrierPSNList': this.paramsPsn,
          'cautionIDList': this.dataForm.cautionId,
          'timeRange': this.dataForm.timeRange,
          'keyWord': this.dataForm.cautionText,
          'pageIndex': this.pageIndex,
          'pageSize': this.pageSize
        })
      }).then(({data}) => {
        if (data && data.code === 0) {
          console.log(data)
          this.cautionDataList = data.page.list
          this.totalPage = data.page.totalCount
        } else {}
        this.dataListLoading = false
      })
    },
    getDateTimeStr (val) {
      this.dataForm.timeRange = val
    },
    handleCurrentChange (val) {
      // 如果选中了记录
      if (val) {
        if (val.historyRecordId !== 0) {
          this.$http({
            url: this.$http.adornUrl('/device/noticeDetail'),
            method: 'get',
            params: this.$http.adornParams({
              'recordId': val.historyRecordId
            }, false)
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.cautionMainBoard = data.info.Mainboard
              this.cautionNetwork = data.info.NetworkInterface
              this.cautionMemory = data.info.MemoryModule
              this.cautionDriver = data.info.Drive
              this.cautionDisplay = data.info.DisplayAdapter
            }
          })
          this.$http({
            url: this.$http.adornUrl('/device/noticeCpuDetail'),
            method: 'get',
            params: this.$http.adornParams({
              'recordId': val.historyRecordId
            }, false)
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.cautionCpu = data.info
            } else {}
          })
          this.detailShowFlag = true
        } else {
          this.$message({
            message: '[下线]类告警无详细告警信息!',
            showClose: true,
            type: 'warning',
            duration: 5000
          })
        }
      }
      console.log('rowContent:' + val.carrierPSN + ' ' + val.historyRecordId)
    },
    // 每页数
    sizeChangeHandle (val) {
      this.pageSize = val
      this.pageIndex = 1
      this.getCautionList()
    },
    // 当前页
    currentChangeHandle (val) {
      this.pageIndex = val
      this.getCautionList()
    }
  }
}
function charData (dataList) {
  var utilValue = 0
  xAxisData.push(dataList[0].timeString)
  for (var i = 0; i < dataList.length; i++) {
    var legName = dataList[i].deviceName
    var arrIndex = legendData.indexOf(legName)
    utilValue = parseFloat(utilValue) + parseFloat(dataList[i].sensorValue)
    if (arrIndex === -1) {
      legendData.push(legName)
      seriesData.push({'name': legName, 'type': 'line', 'smooth': false, 'data': [dataList[i].sensorValue]})
    } else {
      seriesData[arrIndex].data.push(dataList[i].sensorValue)
    }
  }
  normalLineChart('lineChart', 'cpu使用率', JSON.stringify(legendData), '%', JSON.stringify(xAxisData), JSON.stringify(seriesData))
}
function normalLineChart (domId, title, legendData, unitStr, xAxisData, seriesData) {
  var myChart = echarts.init(document.getElementById(domId), 'macarons')
  // 指定图表的配置项和数据
  var option = {
    tooltip: {
      trigger: 'axis'
    },
    grid: {
      left: '1%',
      right: '1%',
      bottom: '15%',
      top: 10,
      containLabel: true
    },
    legend: {
      top: 'bottom',
      data: JSON.parse(legendData),
      textStyle: {
        fontSize: 10
      }
    },
    backgroundColor: 'rgba(128, 128, 128, 0.0)',
    xAxis: {
      type: 'category',
      boundaryGap: false,
      data: JSON.parse(xAxisData)
    },
    yAxis: {
      type: 'value',
      axisLabel: {
        formatter: '{value} ' + unitStr
      }
    },
    series: JSON.parse(seriesData)
  }
  // 使用刚指定的配置项和数据显示图表。
  myChart.setOption(option)
}
</script>