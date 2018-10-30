<template>
  <div style="background-color:#f2f2f2;">
    <mt-header title="服务器告警一览">
      
    </mt-header>
    <div style="padding:0px 10px 5px 10px;border-bottom: 1px solid #eaeef2;background-color:#ffffff;position:fixed;width:100%;top:40px;z-index:99">
      <el-row :gutter='2'>
        <el-col :span="14">
          <div style="margin-top:5px;">
              <p class="p_content_item" style="font-size:14px;"><span>服务器名(编号):</span><span style="margin-left:5px;">{{serverCode}}</span></p>
              <p class="p_content_item" style="font-size:14px;"><span>PSN:</span><span style="margin-left:5px;">{{paramsPsn}}</span></p>
          </div>
        </el-col>
        <el-col :span="10">
          <div style="margin-top:5px;">
              <p class="p_content_item" style="font-size:14px;"><span>机房名:</span><span style="margin-left:5px;">{{roomName}}</span></p>
              <p class="p_content_item" style="font-size:14px;"><span>机柜名:</span><span style="margin-left:5px;">{{frameName}}</span></p>
          </div>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="11">
          <el-input v-model="dataForm.startDate" size="medium" @focus="openPickerStart()" placeholder="选择起始日期" suffix-icon="el-icon-date" clearable></el-input>
        </el-col>
        <el-col :span="2" style="text-align: center;">
          <span style="font-size: 16px;line-height: 36px;">~</span>
        </el-col>
        <el-col :span="11">
          <el-input v-model="dataForm.endDate" size="medium" @focus="openPickerEnd()" placeholder="选择结束日期" suffix-icon="el-icon-date" clearable></el-input>
        </el-col>
      </el-row>
      <el-row :gutter='16' style="margin-top:10px;">
        <el-col :span="12">
          <el-select v-model="dataForm.cautionId" size="medium" multiple placeholder="请选择">
              <el-option
              v-for="item in options"
              :key="item.paramKey"
              :label="item.paramValue"
              :value="item.paramKey">
              </el-option>
            </el-select>
        </el-col>
        <el-col :span="12">
          <el-input v-model="dataForm.cautionText" size="medium" placeholder="告警内容" clearable></el-input>
        </el-col>
      </el-row >
      <el-row :gutter='2' style="margin-top:10px;">
          <span class='detail_serverName' style="float:right;">
            <el-button @click="goBack" size="small" icon="el-icon-back" type="primary" plain>返回</el-button>
          </span>
          <span class='detail_serverName' style="float:right;margin-right:10px;">
            <el-button @click="getCautionList()" size="small" icon="el-icon-search">查询</el-button>
          </span>
        </el-col>
      </el-row>
    </div>
    <div style="margin-top:195px">
      <div>
        <mt-datetime-picker
          type="date"
          ref="pickerStart"
          year-format="{value} 年"
          month-format="{value} 月"
          date-format="{value} 日"
          :startDate="new Date(Date.parse('2016-01-01'.replace(/-/g, '/')))"
          :endDate="defaultEndDate"
          @confirm="handleConfirmStart"
          >
        </mt-datetime-picker>
          <mt-datetime-picker
          type="date"
          ref="pickerEnd"
          year-format="{value} 年"
          month-format="{value} 月"
          date-format="{value} 日"
          :startDate="defaultStartDate"
          :endDate="defaultEndDate"
          @confirm="handleConfirmEnd"
          >
        </mt-datetime-picker>
      </div>
      <mt-loadmore :bottom-method="loadBottom" :bottom-all-loaded="allLoaded" ref="loadmore" :auto-fill="false" :bottom-distance="100" bottom-pull-text="上拉加载更多" bottom-drop-text="加载更多">
        <div style="padding:5px 10px 5px 10px" v-for="item in cautionDataList" >
          <el-card>
              <!-- <div class='div_item_title'>
                  <span class='span_item_title'>计算机名(编号)</span>
              </div> -->
              <div style="margin:-20px 0;">
                <el-row>
                  <el-col :span="24">
                    <div style="height:30px; margin-top:5px;">
                        <p class="p_content_item" style="font-size:14px;"><span>告警内容:</span><span style="margin-left:5px;color:red;">{{item.cautionText}}</span></p>
                    </div>
                  </el-col>
                </el-row>
                <!-- <el-row :gutter='8'>
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
                </el-row> -->
                <el-row :gutter='8'>
                  <el-col :span="24">
                    <div style="height:auto;margin-top:5px;">
                        <p class="p_content_item" style="font-size:14px;"><span>告警时间:</span><span style="margin-left:5px;">{{item.recordTime}}</span></p>
                    </div>
                  </el-col>
                </el-row>
              </div> 
          </el-card>
        </div>
      </mt-loadmore>
    </div>
  </div>
</template>

<script>
  import { Toast } from 'mint-ui'
  export default {
    data () {
      return {
        dataForm: {
          timeRange: ['2016-01-01', '2022-12-31'],
          startDate: '',
          endDate: '',
          cautionId: '',
          cautionText: ''
        },
        pickerVisible: false,
        defaultStartDate: new Date(Date.parse('2016-01-01'.replace(/-/g, '/'))),
        defaultEndDate: new Date(Date.parse('2022-12-31'.replace(/-/g, '/'))),
        serverCode: '',
        paramsPsn: '',
        roomName: '',
        frameName: '',
        allLoaded: false,
        pageIndex: 1,
        pageSize: 10,
        cautionDataList: [],
        options: []
      }
    },
    // created () {
    //   this.getCautionList()
    // },
    methods: {
      openPickerStart () {
        console.log('aaa')
        this.$refs.pickerStart.open()
      },
      handleConfirmStart (data) {
        // let date = moment(data).format('YYYY.MM.DD')
        var dateObj = new Date(data)
        this.defaultStartDate = dateObj
        this.dataForm.startDate = this.dateFtt('yyyy-MM-dd', dateObj)
        this.dataForm.timeRange[0] = this.dataForm.startDate
        console.log(this.dataForm.timeRange)
      },
      openPickerEnd () {
        console.log(this.dataForm.startDate)
        if (this.dataForm.startDate === '') {
          Toast({
            message: '请先选择起始日期',
            position: 'bottom',
            duration: 3000
          })
        } else {
          this.$refs.pickerEnd.open()
        }
      },
      handleConfirmEnd (data) {
        // let date = moment(data).format('YYYY.MM.DD')
        var dateObj = new Date(data)
        this.dataForm.endDate = this.dateFtt('yyyy-MM-dd', dateObj)
        this.dataForm.timeRange[1] = this.dataForm.endDate
        console.log(this.dataForm.timeRange)
      },
      init (item) {
        this.paramsPsn = item.carrierpsn
        this.serverCode = item.serverCode
        this.roomName = item.roomName
        this.frameName = item.frameName
        this.getCautionList()
      },
      getCautionList () {
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
        console.log('aaa' + this.dataForm.timeRange)
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
        })
      },
      goBack () {
        this.$emit('refreshDataList')
      },
      loadBottom () {
        console.log('loadmore')
        this.pageIndex = this.pageIndex + 1
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
            if (data.page.list.length === 0) {
              this.allLoaded = true // 若数据已全部获取完毕
            } else {
              this.cautionDataList = this.cautionDataList.concat(data.page.list)
            }
            this.totalPage = data.page.totalCount
          } else {}
        })
        this.$refs.loadmore.onBottomLoaded()
      },
      dateFtt (fmt, date) {
        var o = {
          'M+': date.getMonth() + 1,
          'd+': date.getDate(),
          'h+': date.getHours(),
          'm+': date.getMinutes(),
          's+': date.getSeconds(),
          'q+': Math.floor((date.getMonth() + 3) / 3),
          'S': date.getMilliseconds()
        }
        if (/(y+)/.test(fmt)) {
          fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length))
        }
        for (var k in o) {
          if (new RegExp('(' + k + ')').test(fmt)) {
            fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? (o[k]) : (('00' + o[k]).substr(('' + o[k]).length)))
          }
        }
        return fmt
      }
    }
  }
</script>

<style>
.detail_serverName {
  margin-left: 5px;
  line-height: 30px;
  font-size: 14px;
  font-weight: bold;
  color: dimgrey;
}
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
    line-height: 30px;height:30px;font-size:14px;overflow: hidden;white-space: nowrap;text-overflow: ellipsis;  
}
.userate_card {
    background-color: #f5dab1;
}
</style>

