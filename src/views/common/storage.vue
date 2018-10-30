<template>
  <div>
    <div v-if="parentShow">
      <mt-header title="存储服务器">
      </mt-header>
      <span>正在开发中</span>
    </div>
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
      // this.getUserInfo()
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
            console.log(this.userInfo)
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

