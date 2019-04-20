<template>
  <div>
    <div class="logoImgBg">
      <img class="logoImg" src="/static/images/user.png">
    </div>
    <div class="divLineOut">
      <div class="nullLine"></div>
      <div class="divLine"></div>
    </div>
    <div class="textAir">
      <text class="textMain">申请获取以下权限</text>
      <text class="textDetail">申请获得个人信息</text>
    </div>
    <div class="nullLine"></div>
    <div>
      <button
        class="login-btn"
        type="primary"
        open-type="getUserInfo"
        lang="zh_CN"
        @getuserinfo="doLogin"
      >一键授权</button>
    </div>
    <div class="all">
      <div class="left"></div>
      <div class="right"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },

  methods: {
    doLogin() {
      console.log("触发");
    }
  },

  created() {
    // let app = getApp()
  }
};
</script>

<style scoped>
.login-btn {
  width: 700rpx;
}

.logoImgBg {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 150rpx;
}

.logoImg {
  width: 200rpx;
  height: 200rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.divLineOut {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.divLine {
  background: #e0e3da;
  width: 80%;
  height: 5rpx;
}

.nullLine {
  height: 120rpx;
}

.textAir {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.textMain {
  height: 70rpx;
  line-height: 70rpx;
  font-size: 40rpx;
}

.textDetail {
  height: 60rpx;
  line-height: 60rpx;
  color: #666;
  padding-left: 10rpx;
  font-size: 30rpx;
}
</style>
>
