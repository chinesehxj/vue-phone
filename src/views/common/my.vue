<template>
  <div>
    <div v-if="parentShow">
    <mt-header title="个人中心">
      
    </mt-header>
    
    <div style="padding:10px; height:100px;">
      <el-row>
        <el-col :span="6">
          <div >
            <img src="~@/assets/img/avatar.png" style="width:80px;height:80px;border-radius: 40px;"/>
          </div>

        </el-col>
        <el-col :span="18">
          <div style="margin-top:10px;">
            <p>
              <span style="font-size:18px;">{{userInfo.realName}}</span>
            </p>
            <p>
              <span style="font-size:14px;">{{userInfo.companyName}}</span>
            </p>
          </div>
        </el-col>
      </el-row>
    </div>
    <div style="height:10px;background-color:#f2f2f2;"></div>
    <div style="border-bottom:1px solid #f2f2f2;">
      <mt-cell title="联系电话">
        <span>{{userInfo.mobile}}</span>
        <i slot="icon" class="el-icon-phone-outline" style="color:#67C23A;"></i>
      </mt-cell>
    </div>
    <div style="border-bottom:1px solid #f2f2f2;">
      <mt-cell title="E-mail">
        <span>{{userInfo.email}}</span>
        <i slot="icon" class="el-icon-message" style="color:#E6A23C;"></i>
      </mt-cell>
    </div>
    <div style="margin-top:20px;padding:10px 20px;">
      <span style="float:right;">
        <el-button type="primary" icon="el-icon-edit-outline" @click.native="updatePasswordHandle(userInfo.username)">修改密码</el-button>
        <el-button type="danger" icon="el-icon-delete" @click.native="logoutHandle()">退出</el-button>
      </span>
    </div>
    </div>

  <update-password v-if="updatePassowrdVisible" ref="updatePassowrd" @refreshDataList="showParent"></update-password>
  </div>
</template>

<script>
  import UpdatePassword from './update-password'
  import { MessageBox } from 'mint-ui'
  export default {
    data () {
      return {
        updatePassowrdVisible: false,
        parentShow: true,
        userInfo: ''
      }
    },
    components: {
      UpdatePassword
    },
    created () {
      this.getUserInfo()
    },
    methods: {
      getUserInfo () {
        this.$http({
          url: this.$http.adornUrl('/sys/user/info'),
          method: 'get',
          params: this.$http.adornParams({}, false)
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.userInfo = data.user
          } else {
          }
        })
      },
      // 修改密码
      updatePasswordHandle (userName) {
        this.parentShow = false
        this.updatePassowrdVisible = true
        this.$nextTick(() => {
          this.$refs.updatePassowrd.init(userName)
        })
      },
      // 显示父组件
      showParent () {
        this.parentShow = true
        this.updatePassowrdVisible = false
      },
      // 退出
      logoutHandle () {
        MessageBox.confirm('确定退出?', '提示').then(() => {
          this.$http({
            url: this.$http.adornUrl('/sys/logout'),
            method: 'post',
            data: this.$http.adornData()
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$cookie.delete('token')
              this.$router.push({ name: 'login' }, () => {
                location.reload() // 刷新页面, 清空整站临时存储数据
              })
            }
          })
        }).catch(() => {})
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

